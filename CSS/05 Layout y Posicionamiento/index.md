---
title: Layout y Posicionamiento — Colocar las cajas en la página
aliases:
  - layout
  - css layout
tags:
  - css
  - api/concepto
  - layout
draft: false
---

# Layout y Posicionamiento

> [!definicion]
> El **layout** es cómo se **colocan** las cajas en la página: una al lado de otra, apiladas, en rejilla, superpuestas. CSS ofrece varios sistemas: el **flujo normal** (por defecto), el **posicionamiento** (`position`), **Flexbox** (una dimensión) y **Grid** (dos dimensiones), además de los **floats** heredados.

```css
.contenedor { display: flex; gap: 1rem; }        /* Flexbox */
.rejilla    { display: grid; grid-template-columns: repeat(3, 1fr); }  /* Grid */
.flotante   { position: sticky; top: 0; }        /* posicionamiento */
```

## Mapa de la sección

- [[01 Flujo Normal del Documento/index]] — `display` block/inline/inline-block/none.
- [[02 Posicionamiento (position)/index]] — `static`, `relative`, `absolute`, `fixed`, `sticky`, `z-index`.
- [[03 Flexbox/index]] — layout en **una** dimensión (filas o columnas).
- [[04 CSS Grid/index]] — layout en **dos** dimensiones (filas y columnas).
- [[05 Multicolumna/index]] — texto repartido en columnas tipo periódico.
- [[06 Floats (legado)/index]] — `float`/`clear`, hoy solo para casos puntuales.

## Cuál usar para qué

> [!tip] La guía rápida de decisión
> | Necesito… | Sistema |
> |-----------|---------|
> | Apilar/alinear elementos en **una fila o columna** | **Flexbox** |
> | Una **rejilla** de filas y columnas | **Grid** |
> | **Superponer** o anclar un elemento (modal, tooltip, sticky) | **position** |
> | Texto en **columnas** tipo periódico | Multicolumna |
> | Que el texto **fluya** alrededor de una imagen | `float` |
>
> La regla general: **Flexbox** para componentes lineales (barras, botoneras, tarjetas), **Grid** para layouts de página y rejillas bidimensionales. Se combinan constantemente (Grid para la página, Flex dentro de cada celda).

## Flexbox vs. Grid: una vs. dos dimensiones

> [!info] La diferencia esencial
> - **Flexbox** organiza en **una** dimensión: una fila **o** una columna. Reparte el espacio a lo largo de un eje. Ideal cuando el contenido manda el tamaño (una barra de navegación, una lista de etiquetas).
> - **Grid** organiza en **dos** dimensiones a la vez: filas **y** columnas, con control explícito de la rejilla. Ideal para layouts estructurados (la maquetación de una página, una galería).
>
> No compiten: se usan juntas. Detalle en [[03 Flexbox/index]] y [[04 CSS Grid/index]].

## Notas relacionadas

- [[03 Flexbox/index]] — el sistema más usado para componentes.
- [[04 CSS Grid/index]] — el más potente para layouts de página.
- [[01 Flujo Normal del Documento/index]] — el comportamiento por defecto, base de todo.
