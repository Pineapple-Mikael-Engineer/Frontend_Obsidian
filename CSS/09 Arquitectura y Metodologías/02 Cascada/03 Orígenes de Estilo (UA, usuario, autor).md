---
title: Orígenes de Estilo — UA, Usuario y Autor
aliases:
  - orígenes de estilo css
  - user-agent stylesheet
  - hoja de estilo del usuario
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 3
---

# Orígenes de Estilo (UA, usuario, autor)

> [!definicion]
> CSS define tres **orígenes de estilo**: el **User-Agent (UA)** (los estilos por defecto del navegador), el **usuario** (hojas de estilo personales del usuario del navegador), y el **autor** (los estilos del desarrollador web). Cada origen tiene su nivel de prioridad en la cascada, y entender su interacción explica por qué existen los resets y cómo funciona la accesibilidad a nivel de CSS.

## Los tres orígenes

### User-Agent (UA)

El navegador trae estilos predeterminados para que el HTML tenga una presentación básica incluso sin CSS del autor. Son los responsables de que `<h1>` sea grande y negrita, `<a>` sea azul y subrayado, `<ul>` tenga viñetas y sangría.

```css
/* Lo que el navegador aplica por defecto (simplificado) */
h1 { font-size: 2em; font-weight: bold; margin: 0.67em 0; }
a { color: -webkit-link; text-decoration: underline; }
ul { list-style-type: disc; padding-inline-start: 40px; }
```

Cada navegador tiene su propia hoja UA (ligeramente distinta), lo que históricamente causó inconsistencias entre navegadores. Por eso existen los CSS resets.

### Usuario

El usuario del navegador puede aplicar su propia hoja de estilo. Aunque hoy es una función poco conocida, fue diseñada para necesidades de accesibilidad: fuentes más grandes, colores de alto contraste, eliminar animaciones. Las extensiones de navegador también pueden actuar en esta capa.

```css
/* Ejemplo de hoja de usuario para accesibilidad */
* { font-size: 18px !important; }
a { color: yellow !important; background: black !important; }
```

### Autor

Los estilos escritos por el desarrollador web. Es la única capa que controla el desarrollador. Incluye todo el CSS que se enlaza con `<link>`, se incrusta con `<style>`, o se aplica con el atributo `style=""`.

## Prioridad en la cascada normal

Para declaraciones normales (sin `!important`), la prioridad es:

```
Autor > Usuario > UA
```

Los estilos del autor sobrescriben los del usuario, que sobrescriben los del UA. Por eso `.btn { color: blue; }` pisa el azul-link por defecto del navegador.

## Prioridad con !important (invertida)

Con `!important`, el orden se invierte en los dos primeros niveles:

```
!important de usuario > !important de autor > !important de UA > autor normal > usuario normal > UA normal
```

Esta inversión es intencional: garantiza que un usuario con necesidades de accesibilidad (que necesita texto más grande o colores de alto contraste) pueda sobrescribir **cualquier decisión del desarrollador**, incluso si este usa `!important`.

```css
/* Hoja de usuario — !important del usuario gana a todo */
* { font-size: 20px !important; }

/* CSS del autor — pierde ante el !important de usuario */
p { font-size: 16px !important; }  /* se aplica 20px del usuario */
```

## Resets y Normalize: herramientas para el origen UA

Los **CSS resets** neutralizan los estilos UA para empezar desde cero, y **Normalize.css** los iguala entre navegadores. Ambos funcionan en la capa de autor (que tiene prioridad sobre UA), sobrescribiendo los defaults.

```css
/* Reset agresivo — borra todo el UA */
*, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

/* Normalize — solo corrige inconsistencias, mantiene lo útil */
/* (más de 400 líneas; carga selectivamente lo necesario) */
```

En proyectos modernos con `@layer`, el reset puede vivir en una capa de baja prioridad, garantizando que cualquier otro estilo lo pueda sobrescribir sin lucha:

```css
@layer reset, base, components, utilities;

@layer reset {
  *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }
}
```

## La capa adicional: @layer

`@layer` añade un nivel de prioridad **dentro del origen de autor**. Las capas se apilan y las declaradas después tienen más prioridad (para estilos normales). Esto permite estructurar los estilos de autor en sub-orígenes controlables, eliminando la necesidad de `!important` para la mayoría de los overrides.

```css
@layer reset, base, components, utilities;
/* utilities > components > base > reset (para estilos normales) */
```

## Cuándo importa el origen en la práctica

La mayoría del tiempo el desarrollador solo trabaja en la capa de autor y no necesita pensar en UA o usuario. El origen importa en tres situaciones:

1. **Inconsistencias entre navegadores** — causadas por diferencias en las hojas UA. Solución: reset o normalize.
2. **Accesibilidad del usuario** — si el usuario fuerza estilos con `!important`, deben poder sobrescribir los del autor. No pongas todo con `!important` o bloquearás la accesibilidad del usuario.
3. **Integración de terceros** — al importar una biblioteca CSS, sus estilos entran en la capa de autor junto a los tuyos. Usar `@layer` para aislar la biblioteca evita colisiones.

## Cómo funciona por dentro

Internamente, el navegador trata cada declaración como un objeto con tres atributos: `(origen, importancia, especificidad)`. El algoritmo de cascada los ordena con esta tupla, evaluando el campo más significativo primero. El campo de origen no es un número simple: es una enum con los seis valores de la tabla de prioridad (tres orígenes × normal/important), ordenados según el spec de CSS.

## Buenas prácticas

> [!tip] Recomendaciones
> - Incluye siempre un reset o normalize para neutralizar las diferencias entre hojas UA.
> - No uses `!important` masivamente: bloquearás la capacidad del usuario de sobrescribir estilos por accesibilidad.
> - Usa `@layer` para estructurar el origen de autor en sub-capas: reset, base, componentes, utilidades.
> - Al integrar CSS de terceros, impórtalo dentro de una `@layer` para aislarlo del resto de tus estilos.

## Errores comunes

> [!warning] Trampas
> - **Ignorar la hoja UA**: los estilos de navegador afectan tu diseño si no los resets/normalizas.
> - **Suponer que el usuario no puede sobrescribir**: las hojas de usuario y las extensiones pueden intervenir, especialmente en contextos de accesibilidad.
> - **Mezclar estilos de terceros con los propios sin @layer**: sus colisiones de especificidad se convierten en las tuyas.

## Notas relacionadas

- [[index]] — la cascada completa.
- [[02 Importancia (!important)]] — cómo !important invierte la prioridad de origen.
- [[03 Capas (@layer)/index]] — sub-capas dentro del origen de autor.
- [[01 Orden de Origen]] — el desempate dentro del mismo origen.
