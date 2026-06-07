---
title: Orden de Origen en la Cascada CSS
aliases:
  - orden de origen
  - source order
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Orden de Origen

> [!definicion]
> El **orden de origen** (source order) es el cuarto y último factor de la cascada: cuando dos declaraciones tienen el mismo origen, la misma capa, y la misma especificidad, **gana la que aparece más tarde** en el CSS. Es el desempate final del algoritmo de la cascada.

```css
/* Mismo elemento, misma especificidad (0,1,0) */
.btn { color: blue; }
.btn { color: red; }   /* gana — viene después */
```

## Cómo funciona en la práctica

El orden de origen aplica tanto dentro de un mismo archivo como entre archivos enlazados. Lo que importa es el orden en que el navegador procesa las declaraciones: primero el `<style>` o `<link>` que aparece antes en el HTML, luego el que viene después.

```html
<!-- styles-base.css se procesa primero -->
<link rel="stylesheet" href="styles-base.css">
<!-- styles-components.css procesa después — sus declaraciones "ganan" en empate -->
<link rel="stylesheet" href="styles-components.css">
```

```css
/* styles-base.css */
.btn { background: gray; border-radius: 4px; }

/* styles-components.css — mismo selector, mismo peso → gana */
.btn { background: blue; }
```

El resultado: `background: blue` (del segundo archivo), `border-radius: 4px` (del primero, no pisado).

## El orden de origen NO reemplaza a la especificidad

El orden de origen solo entra en juego cuando especificidad y capas ya son iguales. Un selector más específico en el primer archivo siempre gana a uno menos específico en el último.

```css
/* Archivo 1 */
#hero .btn { color: red; }    /* (1,1,0) — gana */

/* Archivo 2 — aunque va después, su especificidad es menor */
.btn { color: blue; }         /* (0,1,0) — pierde */
```

## Implicaciones para el orden de los archivos

El orden de importación de hojas de estilo es arquitecturalmente relevante:

1. **Primero**: estilos del navegador / resets (deben ser pisables por todo lo demás).
2. **Después**: estilos base y tokens de diseño.
3. **Después**: componentes.
4. **Al final**: utilidades o temas que deben pisar a los componentes.

Con `@layer` este orden se puede expresar explícitamente en vez de depender del orden físico de los archivos, lo que hace la arquitectura más robusta.

## El orden dentro de un mismo selector

Dentro de un bloque de declaraciones, el orden también importa para propiedades que se solapan (principalmente shorthands vs. longforms):

```css
.btn {
  background: blue;               /* se pisa en la siguiente línea */
  background-color: red;          /* gana — viene después */
}
```

```css
.btn {
  background: blue url(img.png) no-repeat center; /* shorthand */
  background-color: red;          /* sobrescribe solo el color */
}
```

El shorthand establece todas las sub-propiedades; el longform que viene después sobrescribe solo la suya. A la inversa (longform primero, shorthand después), el shorthand resetea todas las sub-propiedades, incluidas las que no se quería cambiar.

## Recetas comunes

### Garantizar que las utilidades ganan sin !important

Colocar las utilidades al final (o en la última `@layer`) les da prioridad de orden sin necesidad de subir su especificidad:

```css
@layer base, components, utilities;

@layer base {
  .btn { color: blue; padding: 0.5rem 1rem; }
}

@layer utilities {
  .text-red { color: red; }   /* gana a .btn en la capa base aunque sea igual de específico */
}
```

### Controlar la sobreescritura de shorthands

Escribir primero el shorthand y después el longform solo si se quiere sobrescribir uno de sus valores:

```css
/* Quiero background azul con imagen repetida, pero color rojo */
.elemento {
  background: blue url(pattern.png);   /* shorthand: imagen + color */
  background-color: red;               /* sobrescribe solo el color */
}
```

## Cómo funciona por dentro

El motor de CSS almacena internamente cada declaración con su posición de origen (un número de orden) además de su especificidad. Cuando llega al paso de desempate, simplemente compara esos números. La posición se asigna durante el análisis del CSS, por lo que los `@import` anidados y los `<style>` inline se interleavan según el orden en que el parser los encuentra.

## Buenas prácticas

> [!tip] Recomendaciones
> - Establece el orden de tus archivos CSS de forma consciente: resets → tokens → componentes → utilidades.
> - Prefiere `@layer` sobre depender del orden de archivos: es explícito y no se rompe si alguien reordena los `<link>`.
> - Pon siempre los shorthands antes que los longforms si quieres que el longform prevalezca.
> - No uses el orden de origen como herramienta principal de cascada: es el último recurso, no el primero.

## Errores comunes

> [!warning] Trampas
> - **Shorthand después del longform**: el shorthand resetea los valores del longform que querías conservar.
> - **Asumir que el último archivo siempre gana**: no si el primero tiene mayor especificidad.
> - **Depender del orden de `@import`**: los `@import` se resuelven antes del CSS restante del archivo que los contiene, lo que puede ser contraintuitivo.

## Notas relacionadas

- [[index]] — el lugar del orden de origen en la cascada completa.
- [[02 Importancia (!important)]] — cómo `!important` salta por encima del orden de origen.
- [[03 Capas (@layer)/index]] — controlar prioridad sin depender del orden de archivos.
- [[01 Especificidad/index]] — el factor que aplica antes del orden de origen.
