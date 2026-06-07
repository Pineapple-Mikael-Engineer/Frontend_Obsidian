---
title: RGB y RGBA — Color por canales rojo, verde y azul
aliases:
  - rgb
  - rgba
tags:
  - css
  - api/funcion
  - fondos
draft: false
---

# RGB y RGBA

> [!definicion]
> La función `rgb()` define un color por sus tres canales **rojo, verde y azul**, con un cuarto opcional de **alfa** (opacidad). Es el mismo modelo que el [[02 Hexadecimal | hexadecimal]], pero con sintaxis numérica más legible y un control de opacidad explícito.

```css
color: rgb(203 166 247);          /* moderno (espacios) */
color: rgb(203 166 247 / 0.5);    /* con alfa */
color: rgba(203, 166, 247, 0.5);  /* heredado (comas) */
```

## La sintaxis: moderna vs. heredada

> [!info] Espacios y / vs. comas
> CSS Color 4 unificó la sintaxis: componentes separados por **espacios** y alfa tras una **barra** `/`. La forma antigua con comas (`rgb()`/`rgba()`) sigue siendo válida:
> ```css
> rgb(203 166 247 / 50%)     /* moderno: espacios + / */
> rgba(203, 166, 247, 0.5)   /* heredado: comas */
> ```
> En la sintaxis moderna, `rgb()` **ya admite alfa**, así que `rgba()` es redundante (se mantiene por compatibilidad). Mismo color, dos escrituras.

## Los valores: 0–255 o porcentajes

Cada canal acepta un número de **0 a 255** o un **porcentaje** de 0 % a 100 %:

```css
rgb(255 0 0)        /* rojo puro */
rgb(100% 0% 0%)     /* lo mismo, en % */
rgb(0 0 0)          /* negro */
rgb(255 255 255)    /* blanco */
rgb(128 128 128)    /* gris medio */
```

| Canal | Rango | Qué controla |
|-------|-------|--------------|
| Rojo | 0–255 | Cantidad de rojo |
| Verde | 0–255 | Cantidad de verde |
| Azul | 0–255 | Cantidad de azul |
| Alfa | 0–1 o 0–100% | Opacidad |

## El canal alfa

El alfa va de `0` (transparente) a `1` (opaco), o en porcentaje:

```css
rgb(0 0 0 / 0.5)    /* negro al 50 % */
rgb(0 0 0 / 50%)    /* idéntico */
```

> [!tip] Alfa más legible que en hex
> Para opacidad, `rgb(... / 0.5)` es mucho más claro que calcular el par hexadecimal (`80`). Si trabajas con transparencias variables (overlays, sombras), `rgb()`/`hsl()` con `/` son preferibles.

## La limitación: difícil de ajustar a mano

> [!warning] RGB no es intuitivo para "aclarar" o "saturar"
> El modelo RGB describe **cuánta luz** de cada canal, no propiedades perceptuales. Para hacer un color "un poco más claro" o "menos saturado", hay que tocar los tres canales a la vez de forma poco intuitiva. Para eso están [[04 HSL y HSLA | HSL]] (tono/saturación/luz) y [[06 LAB y LCH | OKLCH]] (perceptual): permiten ajustar una sola dimensión.

## Ejemplo: overlay sobre una imagen

```css
.overlay {
  background: rgb(30 30 46 / 0.6);   /* tinte oscuro semitransparente */
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `rgb()` cuando necesites alfa legible o leer/ajustar canales.
> - Prefiere la sintaxis moderna (espacios + `/`); `rgba()` es redundante hoy.
> - Para generar variantes de un color (claro/oscuro), usa HSL/OKLCH, no RGB.
> - Guarda los colores en variables, no literales repetidos.

## Errores comunes

> [!warning] Trampas
> - **Mezclar sintaxis**: `rgb(203, 166, 247 / 0.5)` (comas + barra) es inválido; elige una.
> - **Salirse del rango**: valores >255 o >100% se recortan (clamp) al máximo.
> - **Usar RGB para ajustar tonos** cuando HSL/OKLCH sería mucho más simple.

## Notas relacionadas

- [[02 Hexadecimal]] — el mismo modelo, sintaxis compacta.
- [[04 HSL y HSLA]] — para ajustar tono, saturación y luz por separado.
- [[07 color-mix()]] — mezclar colores en vez de calcular canales.
