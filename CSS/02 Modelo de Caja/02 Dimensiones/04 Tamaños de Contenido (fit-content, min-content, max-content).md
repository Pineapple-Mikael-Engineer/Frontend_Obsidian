---
title: Tamaños de Contenido — fit-content, min-content, max-content
aliases:
  - intrinsic sizing
  - min-content
  - max-content
  - fit-content
tags:
  - css
  - api/concepto
  - layout
draft: false
order: 4
---

# Tamaños de Contenido (fit-content, min-content, max-content)

> [!definicion]
> Estas palabras clave dimensionan una caja según su **contenido** (tamaño intrínseco), no según un valor fijo o el contenedor: `max-content` es lo que el contenido pide sin ajustar, `min-content` lo mínimo sin desbordar, y `fit-content` un punto intermedio que se adapta. Se usan en `width`/`height` y, sobre todo, en **CSS Grid**.

```css
.caja { width: max-content; }   /* tan ancha como su contenido sin ajustar */
.celda { width: min-content; }  /* tan estrecha como permita el contenido */
```

## Las tres palabras

| Valor | Ancho resultante |
|-------|------------------|
| `max-content` | El que el contenido necesita **sin ajustar líneas** (texto en una sola línea) |
| `min-content` | El **mínimo** sin desbordar: el ancho de la palabra/elemento más ancho |
| `fit-content` | `max-content`, pero **limitado** al espacio disponible (se ajusta si no cabe) |

## Visualizar con texto

> [!info] Cómo se comporta cada uno con un párrafo
> Imagina un párrafo "Hola mundo cruel":
> - **`max-content`**: la caja se hace tan ancha como toda la frase **en una línea**, sin saltos.
> - **`min-content`**: la caja se encoge al ancho de la **palabra más larga** ("mundo"), apilando el resto verticalmente.
> - **`fit-content`**: se comporta como `max-content` mientras quepa; si no, ajusta como `min-content`.

## fit-content: el más práctico

> [!tip] fit-content para cajas que se ajustan a su contenido
> `fit-content` es muy útil para elementos que deben ser **tan anchos como su contenido** pero **sin pasarse** del contenedor: una etiqueta, un botón, un título con fondo que no debe ocupar toda la línea:
> ```css
> .badge {
>   width: fit-content;       /* solo lo que ocupa el texto */
>   padding: 0.25rem 0.75rem;
>   background: #cba6f7;
> }
> ```
> Sin esto, un elemento de bloque ocuparía todo el ancho aunque el texto sea corto.

## fit-content() como función

Existe también la **función** `fit-content(<límite>)`, que da el tamaño del contenido pero con un tope explícito, muy usada en pistas de [[03 grid-template-columns y rows | Grid]]:

```css
grid-template-columns: fit-content(200px) 1fr;
/* la primera columna se ajusta al contenido, máximo 200px */
```

## Su hogar natural: Grid

Estas palabras brillan en **CSS Grid**, donde definen el tamaño de las pistas según el contenido: una columna `min-content` se encoge al máximo, una `max-content` toma lo que pida el contenido, y `minmax(min-content, max-content)` crea pistas flexibles. Detalle en [[05 minmax() y fit-content()]].

## Buenas prácticas

> [!tip] Recomendaciones
> - `width: fit-content` para que un bloque se ajuste a su contenido sin ocupar toda la línea.
> - `min-content`/`max-content` sobre todo en pistas de Grid.
> - `fit-content(<max>)` cuando quieras "ajustar al contenido, pero con tope".
> - Combínalas con `minmax()` para pistas de grid adaptables.

## Errores comunes

> [!warning] Trampas
> - **Confundir `min`/`max-content`**: uno encoge al máximo, otro expande al contenido.
> - **`max-content` en texto largo**: puede desbordar (no ajusta líneas).
> - **Soporte**: bien soportadas hoy, pero verifica en proyectos con navegadores antiguos.

## Notas relacionadas

- [[01 width y height]] — las dimensiones donde se usan.
- [[05 minmax() y fit-content()]] — su uso en pistas de Grid.
- [[03 grid-template-columns y rows]] — dimensionar columnas por contenido.
