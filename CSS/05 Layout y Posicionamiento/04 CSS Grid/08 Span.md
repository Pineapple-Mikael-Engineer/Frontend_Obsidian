---
title: Span — Que un item ocupe varias celdas
aliases:
  - grid span
tags:
  - css
  - api/concepto
  - layout
draft: false
order: 8
---

# Span

> [!definicion]
> La palabra clave `span` hace que un item de Grid **abarque varias pistas** (columnas o filas) sin tener que indicar las líneas exactas: dices **cuántas** pistas ocupar, no entre qué líneas. Es la forma más cómoda de hacer que un elemento ocupe más de una celda.

```css
.destacado {
  grid-column: span 2;   /* ocupa 2 columnas */
  grid-row: span 2;      /* y 2 filas */
}
```

## span vs. líneas explícitas

> [!info] Cuántas pistas vs. de dónde a dónde
> Dos formas de que un item ocupe varias celdas:
> - **`grid-column: 1 / 3`** (líneas): de la línea 1 a la 3. Fija **dónde** empieza y acaba.
> - **`grid-column: span 2`** (span): ocupa 2 columnas **desde donde le toque** colocarse. No fija la posición de inicio.
>
> `span` es ideal cuando quieres que un item sea "doble de ancho" pero te da igual su posición exacta (que la decida el flujo automático). Las líneas explícitas son para colocar en un sitio concreto.

## Combinar span con una línea de inicio

Se puede fijar el inicio y usar `span` para el tamaño:

```css
grid-column: 2 / span 2;   /* desde la línea 2, abarcando 2 columnas */
grid-column: span 2 / 4;   /* 2 columnas que terminan en la línea 4 */
```

## Recetas con flujo automático

> [!tip] Items destacados en una galería que fluye
> El uso más elegante de `span`: en una galería con colocación automática, marcar algunos items como "grandes" sin posicionarlos a mano. El resto se acomoda solo alrededor:
> ```css
> .galeria { display: grid; grid-template-columns: repeat(4, 1fr); gap: 1rem; }
> .galeria .grande {
>   grid-column: span 2;
>   grid-row: span 2;   /* este item ocupa 2x2; los demás rellenan los huecos */
> }
> ```
> Combinado con [[10 Grid Implícito (auto-rows, auto-columns, auto-flow) | `grid-auto-flow: dense`]], los huecos se rellenan compactando la rejilla.

## Cuidado con span más allá de la rejilla

> [!warning] span puede crear pistas implícitas o desbordar
> Si un item tiene `grid-column: span 3` pero la rejilla solo tiene 2 columnas, el comportamiento depende del eje:
> - En **columnas** (con número fijo): el item se ajusta o desborda según el caso.
> - En **filas**: Grid **crea filas implícitas** para acomodarlo.
>
> Conviene que el `span` sea coherente con el número de pistas disponibles para evitar resultados inesperados.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `span N` cuando importe **cuánto** ocupa un item, no su posición exacta.
> - Combínalo con flujo automático para galerías con items destacados.
> - `grid-auto-flow: dense` para rellenar los huecos que dejan los items grandes.
> - Para posición exacta, usa líneas explícitas en vez de `span`.

## Errores comunes

> [!warning] Trampas
> - **span mayor que las pistas disponibles**: desborda o crea pistas implícitas.
> - **Confundir span con número de líneas**: `span 2` son 2 pistas, no 2 líneas.
> - **Huecos** que dejan los items grandes sin `grid-auto-flow: dense`.

## Notas relacionadas

- [[07 Ubicación por Líneas (grid-column, grid-row)]] — la colocación por líneas, alternativa a span.
- [[10 Grid Implícito (auto-rows, auto-columns, auto-flow)]] — `dense` para rellenar huecos.
- [[06 subgrid]] — donde `span` es requisito.
