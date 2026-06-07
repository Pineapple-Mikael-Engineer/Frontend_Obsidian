---
title: Gradientes — Transiciones de color generadas
aliases:
  - gradients
tags:
  - css
  - api/concepto
  - fondos
draft: false
---

# Gradientes

> [!definicion]
> Un **gradiente** es una transición suave entre colores que CSS genera como una **imagen** (sin archivo). Los hay **lineales** (en una dirección), **radiales** (desde un punto), **cónicos** (alrededor de un centro) y **repetidos**. Se usan en cualquier propiedad que acepte imagen: `background-image`, `border-image`, `mask`.

```css
.a { background: linear-gradient(135deg, #cba6f7, #89dceb); }
.b { background: radial-gradient(circle, #cba6f7, #1e1e2e); }
.c { background: conic-gradient(#cba6f7, #f38ba8, #89dceb, #cba6f7); }
```

## Los tipos

| Tipo | Forma |
|------|-------|
| Lineal | A lo largo de una línea/ángulo |
| Radial | Desde un punto hacia fuera (círculo/elipse) |
| Cónico | Girando alrededor de un centro (como un reloj) |
| Repetidos | Cualquiera de los anteriores, en bucle |

Cada tipo en su nota: [[01 linear-gradient]], [[02 radial-gradient]], [[03 conic-gradient]] y [[04 Gradientes Repetidos]].

## Los color stops (paradas de color)

> [!info] La sintaxis común: colores con posición
> Todos los gradientes comparten el concepto de **color stops**: cada color puede llevar una **posición** (dónde alcanza su pureza). Entre dos stops, el color se interpola:
> ```css
> linear-gradient(to right, red 0%, yellow 50%, green 100%);
> ```
> Dos colores con la **misma** posición crean una **transición dura** (sin degradado), útil para franjas:
> ```css
> linear-gradient(to right, red 50%, blue 50%);   /* mitad roja, mitad azul, sin mezcla */
> ```

## El espacio de interpolación importa

> [!tip] Interpola en oklch para colores vivos
> Como en [[07 color-mix() | `color-mix()`]], el espacio donde se interpola un gradiente cambia el resultado. En `srgb` (por defecto), un gradiente entre colores opuestos puede pasar por un gris turbio; en `oklch`, la transición es más viva. Se especifica con `in`:
> ```css
> linear-gradient(in oklch, blue, yellow);   /* transición vibrante */
> ```

## Ventajas sobre las imágenes

> [!info] Por qué usar gradientes
> Frente a una imagen de fondo:
> - **Cero peso de descarga** (se generan en el navegador).
> - **Escalan** a cualquier tamaño sin pixelarse.
> - **Animables** y combinables (patrones con varios gradientes).
> - Fáciles de tematizar con [[10 Variables CSS/index | variables]].

## Notas relacionadas

- [[01 linear-gradient]] — el más usado.
- [[09 Múltiples Fondos]] — combinar gradientes en capas (overlays, patrones).
- [[07 color-mix()]] — mezcla de dos colores, pariente conceptual.
