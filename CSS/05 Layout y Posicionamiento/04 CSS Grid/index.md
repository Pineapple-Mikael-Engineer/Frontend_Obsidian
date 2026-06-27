---
title: CSS Grid — Layout en dos dimensiones
aliases:
  - css grid
  - grid
tags:
  - css
  - api/concepto
  - layout
draft: false
order: 4
---

# CSS Grid

> [!definicion]
> **CSS Grid** organiza elementos en **dos dimensiones** a la vez: **filas y columnas**, formando una rejilla explícita. A diferencia de [[03 Flexbox/index | Flexbox]] (una dimensión), Grid permite alinear elementos tanto horizontal como verticalmente en una cuadrícula controlada. Es el sistema más potente para **layouts de página** y rejillas estructuradas.

```css
.rejilla {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}
```

## El vocabulario de Grid

> [!info] Pistas, líneas, celdas, áreas
> Grid tiene su propia terminología (detalle en [[02 Conceptos (pistas, líneas, celdas, áreas)]]):
> - **Pista** (track): una fila o columna completa.
> - **Línea** (line): las líneas numeradas que separan las pistas (la base para colocar items).
> - **Celda** (cell): la intersección de una fila y una columna.
> - **Área** (area): un rectángulo de varias celdas.

## Mapa de la subsección

- [[01 Contenedor Grid]] — activar Grid.
- [[02 Conceptos (pistas, líneas, celdas, áreas)]] — el vocabulario.
- [[03 grid-template-columns y rows]] — definir las pistas.
- [[04 Unidad fr y repeat()]] — repartir espacio y repetir pistas.
- [[05 minmax() y fit-content()]] — pistas con límites.
- [[06 subgrid]] — heredar la rejilla del padre.
- [[07 Ubicación por Líneas (grid-column, grid-row)]] — colocar items por líneas.
- [[08 Span]] — que un item ocupe varias celdas.
- [[09 Áreas (grid-template-areas)]] — maquetar con nombres visuales.
- [[10 Grid Implícito (auto-rows, auto-columns, auto-flow)]] — pistas que se crean solas.
- [[11 Alineación en Grid (justify, align, place)]] — alinear items y la rejilla.
- [[12 Patrones (auto-fit, auto-fill)]] — rejillas responsivas automáticas.

## Grid explícito vs. implícito

> [!info] Lo que defines y lo que Grid crea solo
> - **Explícito**: las pistas que defines con `grid-template-columns`/`rows`.
> - **Implícito**: las pistas que Grid **crea automáticamente** cuando hay más items de los que caben en la rejilla explícita (controladas por [[10 Grid Implícito (auto-rows, auto-columns, auto-flow) | `grid-auto-*`]]).

## Las dos formas de colocar items

> [!tip] Dejar fluir o colocar a propósito
> Grid ofrece dos enfoques:
> 1. **Flujo automático**: defines las columnas y los items se colocan solos, llenando la rejilla. Lo más común y simple.
> 2. **Colocación explícita**: sitúas cada item en líneas o áreas concretas ([[07 Ubicación por Líneas (grid-column, grid-row) | por líneas]] o [[09 Áreas (grid-template-areas) | por áreas]]). Para layouts donde cada elemento tiene su sitio.

## Grid vs. Flexbox

| | Grid | Flexbox |
|--|------|---------|
| Dimensiones | **Dos** (filas y columnas) | **Una** (fila o columna) |
| Control | La rejilla manda | El contenido influye |
| Ideal para | Layout de página, galerías | Componentes lineales, barras |

No compiten: Grid para la macro-estructura, Flexbox dentro de las celdas. Se usan juntos constantemente.

## Notas relacionadas

- [[01 Contenedor Grid]] — el punto de partida.
- [[12 Patrones (auto-fit, auto-fill)]] — rejillas responsivas sin media queries.
- [[03 Flexbox/index]] — el sistema unidimensional, complementario.
