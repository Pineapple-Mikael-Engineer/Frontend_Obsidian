---
title: Máscaras y Recortes — Esconder partes de un elemento
aliases:
  - masking and clipping
tags:
  - css
  - api/concepto
  - fondos
draft: false
---

# Máscaras y Recortes

> [!definicion]
> Estas técnicas **ocultan partes** de un elemento para crear formas no rectangulares o efectos de composición: [[01 clip-path | `clip-path`]] recorta con una forma geométrica (todo o nada), [[02 mask | `mask`]] usa una imagen/gradiente para una transparencia gradual, y los [[03 Modos de Mezcla (mix-blend-mode, background-blend-mode) | modos de mezcla]] combinan colores de capas superpuestas.

```css
.hexagono { clip-path: polygon(50% 0, 100% 25%, 100% 75%, 50% 100%, 0 75%, 0 25%); }
.fade { mask: linear-gradient(to bottom, black, transparent); }
```

## clip-path vs. mask

> [!info] Recorte duro vs. transparencia gradual
> La diferencia clave entre las dos técnicas:
> - **`clip-path`**: recorta con una **forma geométrica** definida (polígono, círculo). Es **todo o nada**: cada píxel está dentro (visible) o fuera (oculto), con bordes nítidos.
> - **`mask`**: usa una **imagen o gradiente** como máscara, donde la **luminosidad/alfa** determina la opacidad de cada píxel. Permite transparencias **graduales** (desvanecidos suaves).
>
> | | `clip-path` | `mask` |
> |--|-------------|--------|
> | Bordes | Nítidos | Pueden ser graduales |
> | Define | Una forma | Una imagen/gradiente de opacidad |
> | Uso | Formas geométricas | Desvanecidos, recortes con textura |

## Mapa de la subsección

- [[01 clip-path]] — recortar con formas (círculo, polígono, etc.).
- [[02 mask]] — enmascarar con imágenes y gradientes (transparencia gradual).
- [[03 Modos de Mezcla (mix-blend-mode, background-blend-mode)]] — mezclar colores de capas.

## Notas relacionadas

- [[01 clip-path]] — la más usada para formas.
- [[05 border-radius]] — recorte simple de esquinas (un caso particular).
- [[02 Gráficos Vectoriales (svg)]] — recortes con SVG, parientes de estas técnicas.
