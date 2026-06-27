---
title: Patrones (auto-fit, auto-fill) — Rejillas responsivas automáticas
aliases:
  - auto-fit
  - auto-fill
  - responsive grid
tags:
  - css
  - api/concepto
  - layout
draft: false
order: 12
---

# Patrones (auto-fit, auto-fill)

> [!definicion]
> `auto-fit` y `auto-fill` son palabras clave para [[04 Unidad fr y repeat() | `repeat()`]] que crean rejillas con un **número de columnas automático**: el navegador mete tantas columnas como quepan según el ancho, sin media queries. Combinadas con [[05 minmax() y fit-content() | `minmax()`]], son la receta de galería responsiva por excelencia.

```css
.galeria {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(15rem, 1fr));
  gap: 1rem;
}
```

## La receta responsiva sin media queries

> [!tip] El patrón que todo el mundo memoriza
> Esta línea hace una galería que pasa de 1 a 2 a 3+ columnas según el ancho, automáticamente:
> ```css
> grid-template-columns: repeat(auto-fit, minmax(15rem, 1fr));
> ```
> - `minmax(15rem, 1fr)`: cada columna mide **al menos 15rem**, y crece (`1fr`) para repartir el sobrante.
> - `auto-fit`: crea **tantas columnas como quepan** de ese tamaño.
>
> En una pantalla ancha caben varias columnas; en móvil, una. **Sin una sola media query.** Es la receta de Grid más usada y conviene memorizarla.

## auto-fit vs. auto-fill: la diferencia sutil

> [!warning] La diferencia se nota con pocos items
> Las dos crean columnas automáticas, pero difieren cuando hay **menos items que columnas posibles**:
> - **`auto-fit`**: **colapsa** las columnas vacías a 0 y los items existentes **se estiran** para llenar el ancho.
> - **`auto-fill`**: **mantiene** las columnas vacías (reserva su espacio), así que los items **no se estiran**: quedan a su tamaño con huecos a la derecha.
>
> | Con 2 items y sitio para 4 columnas | Resultado |
> |--|--|
> | `auto-fit` | 2 items que se estiran a media pantalla cada uno |
> | `auto-fill` | 2 items a 15rem, con 2 columnas vacías a la derecha |
>
> **`auto-fit`** suele ser lo deseado (los items llenan el ancho); **`auto-fill`** sirve cuando quieres mantener el "ancho de columna" fijo aunque sobre sitio.

## Evitar el desbordamiento en móvil

> [!warning] El min de minmax puede desbordar
> Si `15rem` es más ancho que la pantalla, la columna no puede encoger y **desborda**. La solución robusta usa `min()`:
> ```css
> grid-template-columns: repeat(auto-fit, minmax(min(15rem, 100%), 1fr));
> ```
> `min(15rem, 100%)` usa 15rem salvo que el contenedor sea más estrecho, evitando el scroll horizontal en móviles pequeños.

## Comparación con la rejilla fluida de Flexbox

| | Grid `auto-fit` | Flexbox `flex-wrap` |
|--|-----------------|---------------------|
| Columnas alineadas | **Sí** (rejilla real) | No (última fila desigual) |
| Última fila | Items del mismo ancho | Items estirados de forma desigual |
| Control | Mayor | Menor |

Para galerías donde las **columnas deben alinearse**, Grid con `auto-fit` es superior a [[03 Ajuste de Línea (flex-wrap) | Flexbox wrap]] (donde la última fila puede quedar descuadrada).

## Buenas prácticas

> [!tip] Recomendaciones
> - Memoriza `repeat(auto-fit, minmax(<min>, 1fr))` para galerías responsivas.
> - Usa `auto-fit` (items se estiran) salvo que quieras columnas de ancho fijo con huecos (`auto-fill`).
> - `minmax(min(<min>, 100%), 1fr)` para evitar el desbordamiento en móvil.
> - Para columnas alineadas en la última fila, Grid es mejor que Flexbox wrap.

## Errores comunes

> [!warning] Trampas
> - **Confundir `auto-fit` y `auto-fill`**: con pocos items, dan resultados muy distintos.
> - **Desbordamiento en móvil** si el `min` supera el ancho de pantalla.
> - **Olvidar `minmax()`**: `repeat(auto-fit, 1fr)` no funciona como rejilla responsiva.

## Notas relacionadas

- [[04 Unidad fr y repeat()]] — `repeat()` y `fr`.
- [[05 minmax() y fit-content()]] — el `minmax()` de la receta.
- [[02 Grid con auto-fit]] — el patrón aplicado en la sección de responsivo.
