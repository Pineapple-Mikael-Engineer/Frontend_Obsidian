---
title: Colores — Los formatos de color en CSS
aliases:
  - css colors
tags:
  - css
  - api/concepto
  - fondos
draft: false
order: 4
---

# Colores

> [!definicion]
> CSS admite varios **formatos** para expresar un color, desde palabras clave (`red`) hasta sistemas numéricos modernos de amplio gamut (`lab()`, `oklch()`). Todos producen un color que el navegador renderiza; difieren en legibilidad, capacidad de manipulación y rango de colores representables.

```css
color: red;                      /* palabra clave */
color: #cba6f7;                  /* hexadecimal */
color: rgb(203 166 247);         /* RGB */
color: hsl(267 89% 81%);         /* HSL */
color: oklch(0.8 0.12 300);      /* OKLCH (amplio gamut) */
```

## Los formatos, de viejo a nuevo

| Formato | Sintaxis | Fuerte en |
|---------|----------|-----------|
| Palabras clave | `red`, `tomato` | Rapidez, prototipo |
| Hexadecimal | `#cba6f7` | Compacto, ubicuo |
| RGB / RGBA | `rgb(203 166 247)` | Control de canales y alfa |
| HSL / HSLA | `hsl(267 89% 81%)` | Ajustar tono/saturación/luz a mano |
| HWB | `hwb(267 65% 3%)` | Mezclar con blanco/negro |
| LAB / LCH / OKLCH | `oklch(0.8 0.12 300)` | Uniformidad perceptual, amplio gamut |
| `color-mix()` | `color-mix(in oklch, …)` | Mezclar dos colores |

Cada formato en su nota: [[01 Palabras Clave de Color]], [[02 Hexadecimal]], [[03 RGB y RGBA]], [[04 HSL y HSLA]], [[05 HWB]], [[06 LAB y LCH]] y [[07 color-mix()]].

## Qué es un color en pantalla

> [!info] Modelos y espacios de color
> Un color en pantalla se define dentro de un **espacio de color**. El clásico es **sRGB** (el de hex, `rgb()`, `hsl()`): el rango de colores que un monitor estándar puede mostrar. Los formatos modernos (`lab()`, `lch()`, `oklch()`) trabajan en espacios **más amplios** (display-p3 y mayores), capaces de representar colores **más vivos** que sRGB no alcanza, en pantallas que lo soportan. Por eso no son solo "otra sintaxis": amplían la paleta disponible.

## La sintaxis moderna sin comas

> [!tip] Espacios en vez de comas, y / para el alfa
> La sintaxis CSS Color 4 separa los componentes con **espacios** (no comas) y el canal alfa con una **barra** `/`:
> ```css
> /* moderno */
> rgb(203 166 247 / 0.5)
> hsl(267 89% 81% / 0.5)
> /* heredado (sigue siendo válido) */
> rgba(203, 166, 247, 0.5)
> ```
> Ambas funcionan; la moderna es la dirección del estándar y unifica todas las funciones de color.

## Mapa de la sección

- [[01 Palabras Clave de Color]] — los nombres (`red`, `transparent`, `currentColor`).
- [[02 Hexadecimal]] — `#rgb`, `#rrggbb`, `#rrggbbaa`.
- [[03 RGB y RGBA]] — canales rojo/verde/azul y alfa.
- [[04 HSL y HSLA]] — tono, saturación, luminosidad.
- [[05 HWB]] — tono + blancura + negrura.
- [[06 LAB y LCH]] — color perceptualmente uniforme y de amplio gamut.
- [[07 color-mix()]] — mezclar dos colores por porcentaje.

## Cuál elegir

> [!tip] Recomendación práctica
> - Para empezar y por compatibilidad universal: **hex** o **`rgb()`**.
> - Para **ajustar** un color a mano (aclarar, saturar) o generar paletas: **`hsl()`** (intuitivo) u **`oklch()`** (uniforme).
> - Para **diseño moderno** con colores vivos y manipulación robusta: **`oklch()`** + **`color-mix()`**.
> - Define los colores como [[10 Variables CSS/index | variables CSS]] y reutilízalos; no repartas literales por toda la hoja.

## Notas relacionadas

- [[02 Hexadecimal]] — el formato más común.
- [[04 HSL y HSLA]] — el más intuitivo para ajustar.
- [[01 background-color]] — donde más se aplican los colores.
