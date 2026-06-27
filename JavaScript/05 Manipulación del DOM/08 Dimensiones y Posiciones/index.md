---
title: Dimensiones y Posiciones — DOM
aliases:
  - dimensiones del DOM
  - posiciones de elementos
tags:
  - javascript
  - api/concepto
  - dom
draft: false
order: 8
---

# Dimensiones y Posiciones

> [!definicion]
> El navegador expone varias familias de propiedades y métodos para leer el tamaño y la posición de los elementos en el documento. Se agrupan por qué incluyen (borde, padding, contenido oculto) y a qué referencial son relativas (viewport, `offsetParent`, documento).

La distinción fundamental:

- `offset*` — incluye padding y border; relativo al `offsetParent`.
- `client*` — incluye padding, excluye border y scrollbar; área visible de contenido.
- `scroll*` — incluye contenido oculto más allá del viewport del elemento.
- `getBoundingClientRect()` — coordenadas exactas relativas al viewport, en punto flotante.
- Posiciones de scroll — desplazamiento actual del documento o del elemento.

## Tabla comparativa

| Propiedad / Método | Incluye border | Incluye padding | Incluye contenido oculto | Relativo a |
|---|---|---|---|---|
| `offsetWidth` / `offsetHeight` | Sí | Sí | No | `offsetParent` |
| `clientWidth` / `clientHeight` | No | Sí | No | — (área visible) |
| `scrollWidth` / `scrollHeight` | No | Sí | Sí | — (contenido total) |
| `getBoundingClientRect()` | Sí | Sí | No | Viewport |
| `scrollTop` / `scrollLeft` | — | — | — | Posición de desplazamiento actual |

## Cómo elegir

Para conocer el **tamaño visual** del elemento incluyendo su borde: `offsetWidth`/`offsetHeight`. Para medir el **área de contenido disponible** (sin borde ni scrollbar): `clientWidth`/`clientHeight`. Para saber si hay **contenido desbordado** o para detectar el final del scroll: `scrollWidth`/`scrollHeight`. Para **posicionar overlays o tooltips** respecto a la pantalla: `getBoundingClientRect()`. Para **controlar o leer el desplazamiento** del documento o de un contenedor: `scrollTop`/`scrollLeft` y los métodos de `window`.

> [!warning]
> Leer `offsetWidth`, `clientHeight`, `getBoundingClientRect()` o `scrollTop` inmediatamente después de modificar el DOM o los estilos fuerza un **reflow síncrono** (layout thrashing). Agrupar siempre las lecturas antes de las escrituras. Ver [[03 Reflows y Repaints]].

## Notas relacionadas

- [[01 offsetWidth y offsetHeight]]
- [[02 clientWidth y clientHeight]]
- [[03 scrollWidth y scrollHeight]]
- [[04 getBoundingClientRect]]
- [[05 Posiciones de Scroll]]
