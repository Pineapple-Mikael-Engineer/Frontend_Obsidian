---
title: Gráficos — Dibujo en el navegador (canvas y svg)
aliases:
  - graphics
  - gráficos html
tags:
  - html
  - api/concepto
  - multimedia
draft: false
---

# Gráficos

> [!definicion]
> Más allá de insertar imágenes, HTML puede **dibujar** gráficos generados: [[01 Lienzo (canvas) | `<canvas>`]] ofrece un lienzo de píxeles que se pinta con JavaScript, y [[02 Gráficos Vectoriales (svg) | `<svg>`]] describe formas vectoriales con etiquetas. Son dos enfoques opuestos para generar imágenes en vez de cargarlas.

```html
<canvas id="lienzo" width="400" height="300"></canvas>

<svg viewBox="0 0 100 100" width="100">
  <circle cx="50" cy="50" r="40" fill="#cba6f7" />
</svg>
```

## canvas vs. svg: el contraste fundamental

| | `<canvas>` | `<svg>` |
|--|-----------|---------|
| Modelo | Píxeles (raster) | Vectorial (formas) |
| Cómo se dibuja | Con JavaScript (API de contexto) | Con etiquetas (o JS) |
| Escalado | Pierde nitidez al ampliar | Escala perfecto |
| Elementos individuales | No (todo es un mapa de píxeles) | Sí (cada forma es un nodo del DOM) |
| Ideal para | Juegos, muchos objetos, manipulación de píxeles | Iconos, logos, diagramas, gráficos |
| Accesibilidad | Difícil (necesita respaldo) | Buena (`<title>`, texto en el DOM) |

> [!info] La regla práctica
> - **Pocas formas, escalables, accesibles** (un logo, un icono, un gráfico de barras) → **SVG**.
> - **Muchísimos objetos cambiando rápido, o manipulación de píxeles** (un juego, un editor de imágenes, una visualización con miles de puntos) → **canvas**.

## Mapa de la sección

- [[01 Lienzo (canvas)]] — el lienzo de píxeles pintado con JavaScript.
- [[02 Gráficos Vectoriales (svg)]] — los gráficos vectoriales descritos con etiquetas.

## El dibujo se delega a JavaScript (canvas)

> [!info] canvas es solo el lienzo
> El elemento `<canvas>` por sí solo es un rectángulo en blanco: **todo el dibujo** (líneas, formas, imágenes, animación) se hace con la **Canvas API** desde JavaScript. Por eso el detalle de cómo dibujar pertenece al curso de JS; aquí se documenta el elemento HTML y su papel.

## Notas relacionadas

- [[01 Lienzo (canvas)]] · [[02 Gráficos Vectoriales (svg)]] — las dos técnicas.
- [[04 Formatos de Imagen]] — SVG dentro del panorama de formatos.
