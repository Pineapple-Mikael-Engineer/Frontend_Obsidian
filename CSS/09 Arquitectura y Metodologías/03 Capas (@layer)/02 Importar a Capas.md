---
title: "Importar a Capas (@import layer)"
aliases:
  - "@import layer"
  - importar css a capa
tags:
  - css
  - api/concepto
  - arquitectura
propiedad: "@import"
grupo: at-rule
draft: false
order: 2
---

# Importar a Capas

> [!definicion]
> `@import` acepta una cláusula `layer()` que asigna el archivo importado directamente a una capa de cascada. Esto permite integrar CSS de terceros (o módulos propios) dentro de la jerarquía de capas, controlando su prioridad sin modificar el archivo importado.

```css
/* Importar una biblioteca dentro de una capa de baja prioridad */
@import url("reset.css") layer(reset);
@import url("normalize.css") layer(reset);
@import url("bootstrap.css") layer(third-party);

/* El CSS de la app — sin capa — gana sobre todo lo anterior */
.btn { color: var(--color-primary); }
```

## Sintaxis

```css
/* Forma básica */
@import url("archivo.css") layer;                   /* capa anónima */
@import url("archivo.css") layer(nombre);           /* capa con nombre */

/* Con condición de media query */
@import url("print.css") layer(print) print;

/* Con supports */
@import url("grid.css") layer(layout) supports(display: grid);
```

La cláusula `layer` puede ir sola (capa anónima) o con nombre entre paréntesis. El nombre debe coincidir con el de las capas ya declaradas en el archivo que importa.

## Por qué importa para las bibliotecas de terceros

Sin `@layer`, una biblioteca como Bootstrap tiene sus propios selectores con cierta especificidad. Si tu CSS no tiene mayor especificidad, pierde. Con `@layer`, el CSS de la biblioteca queda en una capa de menor prioridad que el tuyo, sin necesidad de `!important`:

```css
/* Sin @layer — colisión de especificidad con Bootstrap */
/* Bootstrap usa .btn con (0,1,0) */
.btn { background: red; }   /* puede perder si Bootstrap tiene más especificidad */

/* Con @layer — Bootstrap queda en capa de baja prioridad */
@layer reset, bootstrap, components;
@import url("bootstrap.css") layer(bootstrap);

@layer components {
  .btn { background: red; }   /* gana sobre bootstrap.css aunque tenga igual especificidad */
}
```

## Declarar el orden antes de importar

Es una buena práctica declarar el orden de todas las capas antes de los `@import`. Esto garantiza que el orden de prioridad queda claro al inicio del archivo y no depende del orden de las importaciones:

```css
/* main.css */
@layer reset, third-party, base, components, utilities;

@import url("css-reset.css") layer(reset);
@import url("bootstrap.css") layer(third-party);

@layer base { /* ... */ }
@layer components { /* ... */ }
@layer utilities { /* ... */ }
```

## Combinar con @import de módulos propios

Los archivos propios también pueden importarse a capas. Esto es útil en proyectos con muchos archivos para que el orden lógico de las capas esté en un solo lugar (el punto de entrada), independientemente del orden físico de los archivos:

```css
/* punto-de-entrada.css */
@layer base, layout, components, pages, utilities;

@import url("base/reset.css") layer(base);
@import url("base/typography.css") layer(base);
@import url("layout/grid.css") layer(layout);
@import url("components/buttons.css") layer(components);
@import url("pages/home.css") layer(pages);
@import url("utilities/spacing.css") layer(utilities);
```

## La interacción con !important dentro de las capas

Si el CSS importado usa `!important` dentro de su capa, ese `!important` puede ser difícil de sobrescribir. La regla es: los `!important` de capas **anteriores** ganan sobre los de capas posteriores. Así que si una biblioteca en `third-party` usa `!important`, solo otro `!important` en `reset` (anterior) o un `!important` fuera de capas puede ganarle.

```css
@layer reset, third-party;

@layer third-party {
  .btn { color: red !important; }   /* difícil de sobrescribir */
}

/* Para vencerlo desde fuera de capas: */
.btn { color: blue !important; }    /* gana — !important fuera de capas */
```

## Limitaciones

- `@import` debe aparecer antes de cualquier regla que no sea `@charset` o `@layer`. Si hay estilos antes, el `@import` se ignora.
- `@import` es bloqueante para el render: cada importación es una petición HTTP adicional. En producción, es mejor usar un bundler (Vite, webpack) para consolidar los archivos.
- Las capas anónimas (sin nombre) no pueden recibir más estilos después de crearse: son de un solo bloque.

## Recetas comunes

### Aislar Tailwind en una capa

Tailwind puede importarse en una capa para que los estilos de componente puedan sobrescribirlo:

```css
@layer tailwind, components;

@import "tailwindcss" layer(tailwind);

@layer components {
  .card { padding: 1.5rem; }   /* gana sobre las utilidades de Tailwind en esa capa */
}
```

### Reset dentro de capa como primera pieza

```css
@layer reset, base, components;

@layer reset {
  *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }
}

/* O importando un reset externo: */
@import url("modern-normalize.css") layer(reset);
```

## Cómo funciona por dentro

Cuando el parser encuentra `@import url(...) layer(nombre)`, encola la petición HTTP del archivo y, al recibir la respuesta, procesa su CSS como si cada declaración estuviera dentro de un bloque `@layer nombre { }`. La resolución del orden de capas ocurre después de que todos los archivos se han cargado y parseado, usando el índice de capas que se fijó al inicio.

## Buenas prácticas

> [!tip] Recomendaciones
> - Declara el orden de capas en una sola línea al inicio del archivo de entrada antes de los `@import`.
> - Importa siempre el CSS de terceros a una capa dedicada para aislar su especificidad.
> - Evita `@import` en cadena en producción; usa un bundler para consolidar las peticiones.
> - Nombra las capas de terceros de forma que describan su rol, no la biblioteca: `vendor`, `third-party`, `lib`.

## Errores comunes

> [!warning] Trampas
> - **@import después de reglas CSS**: el `@import` se ignora; debe ir primero.
> - **Confundir que !important de capa posterior no gana**: en capas, !important es al revés (capa anterior gana).
> - **Importar a capa anónima y esperar poder añadir más estilos después**: una vez cerrada, no se puede acumular.

## Notas relacionadas

- [[index]] — @layer en contexto.
- [[01 Definición y Orden de Capas]] — cómo se declaran y priorizan las capas.
- [[02 Importancia (!important)]] — cómo !important interactúa con la prioridad de capas.
- [[03 Orígenes de Estilo (UA, usuario, autor)]] — las capas son un sub-nivel del origen de autor.
