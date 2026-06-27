---
title: line-clamp — Limitar el texto a N líneas
aliases:
  - line-clamp
  - -webkit-line-clamp
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: line-clamp
grupo: tipografia
valor_inicial: none
hereda: false
animable: false
draft: false
order: 4
---

# line-clamp

> [!definicion]
> `line-clamp` (mayormente `-webkit-line-clamp`) limita un bloque de texto a un **número de líneas** concreto, recortando lo que sobra y añadiendo puntos suspensivos (`…`). Es la forma de crear resúmenes de altura uniforme (en tarjetas, listados) que no se desbordan.

```css
.resumen {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

## La receta (con sus tres líneas mágicas)

> [!warning] line-clamp necesita un trío de propiedades
> A diferencia de [[05 text-overflow | `text-overflow: ellipsis`]] (una línea), recortar a **varias** líneas requiere una combinación heredada de WebKit con tres declaraciones que deben ir juntas:
> ```css
> .clamp {
>   display: -webkit-box;          /* contexto de caja flexible antigua */
>   -webkit-box-orient: vertical;  /* apilar las líneas */
>   -webkit-line-clamp: 3;         /* número de líneas */
>   overflow: hidden;              /* ocultar lo que sobra */
> }
> ```
> Falta cualquiera de las tres y no funciona. Pese al prefijo `-webkit-`, está soportada en todos los navegadores modernos.

## La propiedad moderna line-clamp

> [!info] line-clamp sin prefijo, en camino
> Existe una propiedad estandarizada `line-clamp` (sin `-webkit-`) que no requiere el `display: -webkit-box`, más limpia. Su soporte está creciendo; mientras tanto, la receta con `-webkit-` sigue siendo la compatible. Conviene conocer ambas y usar la `-webkit-` por compatibilidad hoy.

## Casos de uso

| Uso | Líneas |
|-----|--------|
| Título de tarjeta | 1–2 |
| Resumen/extracto de artículo | 2–4 |
| Descripción de producto en rejilla | 2–3 |

El objetivo: que todas las tarjetas de una rejilla tengan la **misma altura** de texto, independientemente de cuánto contenido tengan.

## Los puntos suspensivos automáticos

`line-clamp` añade `…` automáticamente al final de la última línea visible cuando el texto se recorta. No hace falta `text-overflow` (esa es para una línea con `nowrap`).

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa la receta de 3 propiedades `-webkit-` para recortar a N líneas.
> - Ideal para resúmenes y títulos de altura uniforme en listados/rejillas.
> - Asegúrate de que el texto completo siga accesible (no lo elimines del DOM, solo se oculta visualmente).
> - Considera mostrar el texto completo en hover o con un "ver más".

## Errores comunes

> [!warning] Trampas
> - **Faltar una de las 3 propiedades**: no recorta.
> - **Esperar `text-overflow: ellipsis`** para varias líneas: esa es solo para una.
> - **Recortar contenido importante** sin forma de verlo completo.

## Notas relacionadas

- [[05 text-overflow]] — recorte de **una** línea con `…`.
- [[13 white-space]] — el control del ajuste de líneas.
- [[02 Grid con auto-fit]] — las rejillas de tarjetas donde line-clamp da altura uniforme.
