---
title: Contenedor Grid — Activar CSS Grid
aliases:
  - display grid
  - grid container
tags:
  - css
  - api/propiedad
  - layout
propiedad: display
grupo: grid
valor_inicial: inline
hereda: false
animable: false
draft: false
---

# Contenedor Grid

> [!definicion]
> `display: grid` convierte un elemento en **contenedor grid**: sus hijos directos se vuelven **items grid** y se colocan en una rejilla de filas y columnas. Casi siempre se acompaña de [[03 grid-template-columns y rows | `grid-template-columns`]] para definir cuántas columnas tiene.

```css
.rejilla {
  display: grid;
  grid-template-columns: repeat(3, 1fr);   /* 3 columnas iguales */
  gap: 1rem;
}
```

## El contenedor mínimo

> [!info] display: grid + columns + gap
> Tres declaraciones cubren la mayoría de rejillas:
> ```css
> .grid {
>   display: grid;
>   grid-template-columns: repeat(3, 1fr);   /* cuántas columnas y su tamaño */
>   gap: 1rem;                                /* espacio entre celdas */
> }
> ```
> Sin `grid-template-columns`, Grid pone todos los items en **una sola columna** (una fila por item), lo que rara vez es lo buscado. Por eso definir las columnas es casi obligatorio.

## grid vs. inline-grid

| Valor | El contenedor es… |
|-------|-------------------|
| `grid` | Un bloque (ocupa todo el ancho) |
| `inline-grid` | En línea (fluye con el texto) |

`grid` para la mayoría; `inline-grid` para una rejilla que convive con texto en la misma línea (raro).

## Solo afecta a los hijos directos

Como Flexbox, `display: grid` convierte en items solo a los **hijos directos**. Los nietos no participan en la rejilla salvo que su padre sea también grid (o se use [[06 subgrid | `subgrid`]]).

## Ejemplos de uso

```css
/* Galería de 3 columnas */
.galeria { display: grid; grid-template-columns: repeat(3, 1fr); gap: 1rem; }

/* Layout de página: barra lateral + contenido */
.pagina { display: grid; grid-template-columns: 250px 1fr; gap: 2rem; }

/* Rejilla responsiva automática */
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(15rem, 1fr));
  gap: 1rem;
}

/* Centrar un único elemento (place-items) */
.centro { display: grid; place-items: center; }
```

## El centrado más corto

> [!tip] place-items: center, el centrado mínimo
> Grid ofrece el centrado más compacto de CSS:
> ```css
> .centro { display: grid; place-items: center; }
> ```
> `place-items: center` centra el contenido en ambos ejes a la vez (combina `align-items` y `justify-items`). Más corto que las dos líneas de Flexbox.

## Buenas prácticas

> [!tip] Recomendaciones
> - `display: grid` + `grid-template-columns` + `gap` es la base de casi toda rejilla.
> - Sin definir columnas, Grid apila en una sola columna (rara vez lo deseado).
> - `place-items: center` para el centrado más corto.
> - Para rejillas responsivas, `repeat(auto-fit, minmax(...))` (ver [[12 Patrones (auto-fit, auto-fill)]]).

## Errores comunes

> [!warning] Trampas
> - **Olvidar `grid-template-columns`**: todo queda en una columna.
> - **Esperar que afecte a los nietos**: solo a hijos directos (usa `subgrid` si hace falta).
> - **Usar Grid para componentes lineales** simples: Flexbox suele bastar.

## Notas relacionadas

- [[03 grid-template-columns y rows]] — definir las pistas.
- [[04 Unidad fr y repeat()]] — `1fr` y `repeat()`.
- [[11 Alineación en Grid (justify, align, place)]] — `place-items` y alineación.
