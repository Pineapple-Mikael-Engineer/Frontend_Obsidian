---
title: Fondos y Efectos Visuales — Pintar más allá del contenido
aliases:
  - backgrounds and effects
tags:
  - css
  - api/concepto
  - fondos
draft: false
---

# Fondos y Efectos Visuales

> [!definicion]
> Esta sección cubre cómo **pintar** una caja más allá de su contenido: **fondos** (colores, imágenes, gradientes), **sombras** (`box-shadow`), **recortes y máscaras** (`clip-path`, `mask`), **filtros** (`filter`, `backdrop-filter`) y efectos de borde (`outline`). Son las herramientas que dan profundidad, textura y carácter visual a la interfaz.

```css
.tarjeta {
  background: linear-gradient(135deg, #cba6f7, #89dceb);
  box-shadow: 0 4px 12px rgb(0 0 0 / 0.15);
  border-radius: 1rem;
}
```

## Mapa de la sección

- [[01 Propiedades de Fondo/index]] — color, imagen, repetición, posición, tamaño, fondos múltiples.
- [[02 Gradientes/index]] — lineales, radiales, cónicos y repetidos.
- [[03 Sombras (box-shadow)]] — sombras de caja (profundidad, elevación, glow).
- [[04 Máscaras y Recortes/index]] — `clip-path`, `mask` y modos de mezcla.
- [[05 Filtros/index]] — `filter` (blur, brillo…) y `backdrop-filter` (cristal esmerilado).
- [[06 Efectos de Borde/index]] — `outline` y `box-decoration-break`.

## El fondo es una pila de capas

> [!info] Varias capas de fondo en un elemento
> Un mismo elemento puede tener **varias capas de fondo** apiladas (varios gradientes, una imagen sobre un color), declaradas en una sola propiedad `background` separadas por comas. La primera capa va **encima**. Esto permite efectos ricos —un degradado semitransparente sobre una foto, patrones combinados— sin elementos extra. Detalle en [[09 Múltiples Fondos]].

## Rendimiento de los efectos

> [!warning] Algunos efectos son caros de pintar
> Sombras grandes con mucho desenfoque, `filter: blur()`, `backdrop-filter` y máscaras complejas tienen un **coste de renderizado** real, sobre todo si se animan o se aplican a muchos elementos. Conviene usarlos con mesura y, al animar, preferir `transform`/`opacity` (ver [[03 Animar transform y opacity]]). Un diseño bonito que va a tirones es peor que uno sobrio y fluido.

## Notas relacionadas

- [[01 Propiedades de Fondo/index]] — el punto de partida.
- [[02 Gradientes/index]] — fondos generados sin imágenes.
- [[05 border-radius]] — el redondeo que recorta los fondos.
