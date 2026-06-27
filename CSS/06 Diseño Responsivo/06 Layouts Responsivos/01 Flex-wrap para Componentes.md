---
title: Flex-wrap para Componentes — Reflujo automático con Flexbox
aliases:
  - flex-wrap responsive
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 1
---

# Flex-wrap para Componentes

> [!definicion]
> El patrón **flex-wrap** crea componentes que se **reflujan** automáticamente: una fila de elementos que, cuando no caben, saltan a la siguiente línea, sin media queries. Es ideal para barras de botones, listas de etiquetas y grupos de campos que deben adaptarse al ancho.

```css
.acciones {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}
```

## El patrón básico

> [!tip] flex-wrap: wrap + gap
> La receta mínima: un contenedor flex con `wrap` y `gap`. Los items se ponen en fila mientras caben y saltan de línea cuando no:
> ```css
> .botonera {
>   display: flex;
>   flex-wrap: wrap;   /* salta de línea si no caben */
>   gap: 0.5rem;       /* separación uniforme, también entre filas */
> }
> ```
> En escritorio, los botones van en una fila; en móvil, se apilan en varias. Sin una sola media query.

## La rejilla fluida con flex

Combinando `flex-wrap` con [[09 flex-grow, flex-shrink, flex-basis | `flex: 1 1 <ancho>`]], los items forman una rejilla que ajusta el número de columnas al ancho:

```css
.galeria { display: flex; flex-wrap: wrap; gap: 1rem; }
.galeria > * {
  flex: 1 1 15rem;   /* ancho ideal 15rem; crecen, encogen y saltan */
}
```

## flex-wrap vs. Grid auto-fit

> [!info] Cuándo flex-wrap y cuándo grid
> Ambos crean rejillas responsivas, pero difieren en un punto clave:
> - **`flex-wrap`**: la **última fila** puede quedar **desigual** (los items se estiran para llenar el ancho restante, así que la última fila tiene items más anchos).
> - **`grid` con [[02 Grid con auto-fit | `auto-fit`]]**: las columnas están **alineadas** en una rejilla real; la última fila mantiene el ancho de columna.
>
> Para botones y etiquetas (donde la desigualdad no importa), `flex-wrap` es perfecto y simple. Para galerías donde las **columnas deben alinearse**, Grid es mejor.

## Recetas

```css
/* Barra de botones que se apila en móvil */
.acciones { display: flex; flex-wrap: wrap; gap: 0.5rem; }

/* Etiquetas/chips que fluyen */
.tags { display: flex; flex-wrap: wrap; gap: 0.5rem; }

/* Campos de formulario que pasan de fila a columna */
.fila-form { display: flex; flex-wrap: wrap; gap: 1rem; }
.fila-form > * { flex: 1 1 12rem; }

/* Navbar: logo + menú que envuelve si no cabe */
.navbar { display: flex; flex-wrap: wrap; justify-content: space-between; gap: 1rem; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - `flex-wrap: wrap` + `gap` para componentes que se apilan en móvil.
> - `flex: 1 1 <ancho>` para rejillas fluidas donde la desigualdad de la última fila no importa.
> - Para columnas alineadas (galerías), usa Grid `auto-fit`.
> - `flex-shrink: 0` en items que no deben encogerse (iconos, avatares).

## Errores comunes

> [!warning] Trampas
> - **Olvidar `wrap`**: los items se aprietan o desbordan en móvil.
> - **Última fila desigual** donde se esperaban columnas alineadas: usa Grid.
> - **Sin `gap`**: hacks de márgenes en su lugar.

## Notas relacionadas

- [[03 Ajuste de Línea (flex-wrap)]] — la propiedad `flex-wrap`.
- [[02 Grid con auto-fit]] — la alternativa con columnas alineadas.
- [[10 Casos de Uso]] — más recetas de Flexbox.
