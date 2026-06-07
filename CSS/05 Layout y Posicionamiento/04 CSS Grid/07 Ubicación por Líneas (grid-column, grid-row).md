---
title: Ubicación por Líneas — grid-column y grid-row
aliases:
  - grid-column
  - grid-row
  - grid placement
tags:
  - css
  - api/propiedad
  - layout
propiedad: grid-column
grupo: grid
valor_inicial: auto
hereda: false
animable: false
draft: false
---

# Ubicación por Líneas (grid-column, grid-row)

> [!definicion]
> `grid-column` y `grid-row` colocan un **item** en la rejilla indicando entre qué **líneas** se extiende. Permiten situar cada elemento exactamente donde se quiere, hacer que ocupe varias celdas, o que se solape con otros. Es la colocación **explícita** de Grid.

```css
.destacado {
  grid-column: 1 / 3;   /* de la línea 1 a la 3 = ocupa 2 columnas */
  grid-row: 1 / 2;
}
```

## La sintaxis: de una línea a otra

```css
grid-column: <línea-inicio> / <línea-fin>;
```

| Valor | Significa |
|-------|-----------|
| `grid-column: 1 / 3` | De la línea 1 a la 3 (2 columnas) |
| `grid-column: 2 / -1` | De la línea 2 hasta la **última** |
| `grid-column: 1 / span 2` | Desde la línea 1, abarcando 2 pistas |
| `grid-row: 2` | Solo la línea de inicio (ocupa 1 celda) |

## Piensa en líneas, no en columnas

> [!info] De la línea X a la Y
> El item se coloca **entre líneas**, no "en la columna N". Una rejilla de 3 columnas tiene 4 líneas (1, 2, 3, 4). `grid-column: 1 / 3` significa "desde la línea 1 hasta la línea 3", ocupando las dos primeras columnas. Este modelo de líneas es lo que da a Grid su flexibilidad:
> ```css
> .ancho { grid-column: 1 / -1; }   /* de la primera a la última línea = todo el ancho */
> ```

## La línea -1: hasta el final

> [!tip] -1 es la última línea, sin contar
> La línea `-1` es siempre la **última** de la rejilla, contando desde el final. `grid-column: 1 / -1` hace que un item ocupe **todo el ancho** sin importar cuántas columnas haya. Es muy útil para una cabecera o pie que abarca toda la rejilla:
> ```css
> .cabecera { grid-column: 1 / -1; }   /* fila completa */
> ```

## El shorthand grid-area

`grid-area` combina las cuatro líneas (fila-inicio / columna-inicio / fila-fin / columna-fin):

```css
grid-area: 1 / 1 / 3 / 4;
/*         row-start / col-start / row-end / col-end */
```

(También sirve para referirse a un área nombrada; ver [[09 Áreas (grid-template-areas)]].)

## Líneas con nombre

Si las pistas se definieron con líneas nombradas, se pueden usar los nombres en vez de números:

```css
grid-template-columns: [inicio] 1fr [medio] 2fr [fin];
.item { grid-column: inicio / fin; }
```

## Recetas comunes

```css
/* Un elemento "destacado" que ocupa 2 columnas y 2 filas */
.feature { grid-column: span 2; grid-row: span 2; }

/* Cabecera y pie a todo el ancho, contenido en medio */
.header, .footer { grid-column: 1 / -1; }

/* Item en una posición concreta de la rejilla */
.especial { grid-column: 2 / 4; grid-row: 1 / 3; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Piensa en **líneas** (de X a Y), no en columnas.
> - `1 / -1` para ocupar todo el ancho sin contar columnas.
> - Usa `span N` cuando importe **cuántas** pistas ocupar, no las líneas exactas.
> - Para layouts complejos legibles, considera [[09 Áreas (grid-template-areas) | áreas nombradas]].

## Errores comunes

> [!warning] Trampas
> - **Contar columnas en vez de líneas**: `grid-column: 1 / 3` son 2 columnas, no 3.
> - **Olvidar que `-1` es la última línea**: práctico para "hasta el final".
> - **Solapamientos** no deseados al colocar items en las mismas celdas.

## Notas relacionadas

- [[02 Conceptos (pistas, líneas, celdas, áreas)]] — el modelo de líneas.
- [[08 Span]] — abarcar varias pistas.
- [[09 Áreas (grid-template-areas)]] — la alternativa visual con nombres.
