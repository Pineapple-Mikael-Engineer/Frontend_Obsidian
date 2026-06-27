---
title: Partes del Modelo de Caja — Contenido, padding, border, margin
aliases:
  - box model parts
tags:
  - css
  - api/concepto
  - layout
draft: false
order: 1
---

# Partes del Modelo de Caja

> [!definicion]
> Cada caja CSS tiene cuatro áreas concéntricas: el **área de contenido** (donde van el texto y los hijos), el **padding** (relleno entre contenido y borde), el **border** (la línea del borde) y el **margin** (espacio exterior que separa de otras cajas). Cada una se comporta de forma distinta respecto al fondo, los eventos y el colapso.

```css
.caja {
  width: 200px;          /* área de contenido */
  padding: 16px;         /* relleno */
  border: 2px solid #888;/* borde */
  margin: 24px;          /* margen exterior */
}
```

## Las cuatro áreas en detalle

| Área | Qué contiene | ¿Recibe el fondo? | ¿Recibe eventos? |
|------|--------------|-------------------|------------------|
| Contenido | Texto, imágenes, hijos | Sí | Sí |
| Padding | Espacio interior transparente | **Sí** (lo pinta el `background`) | Sí (hover, clic) |
| Border | La línea del borde | El borde tiene su propio color | Sí |
| Margin | Espacio exterior | **No** (siempre transparente) | No |

## Padding vs. margin: la diferencia clave

> [!info] Dentro vs. fuera del borde
> La distinción fundamental:
> - **Padding** está **dentro** del borde: separa el contenido del borde, **recibe el color de fondo** y los clics. Úsalo para dar "aire interior" a un elemento (el espacio dentro de un botón).
> - **Margin** está **fuera** del borde: separa el elemento de sus vecinos, es **siempre transparente** y no recibe eventos. Úsalo para el espacio **entre** elementos.
>
> Regla rápida: *padding agranda el elemento por dentro; margin lo aleja de los demás*.

## El fondo llega hasta el borde

El `background` de un elemento pinta el **contenido y el padding** (hasta el borde exterior, según [[07 background-clip y background-origin | `background-clip`]]). Por eso el padding "se ve" del color de fondo, mientras que el margin nunca: es espacio vacío que deja ver lo que hay detrás.

## El margin no recibe eventos

> [!warning] El hover no se activa en el margin
> Como el margin es espacio exterior, pasar el ratón por encima **no** dispara `:hover` del elemento, y los clics ahí no le llegan. Si necesitas un área clicable más grande (un botón con zona de toque amplia), usa **padding**, no margin: el padding sí forma parte del elemento.

## Las cajas lógicas (inline/block)

CSS moderno nombra los lados de forma **lógica** además de física, para soportar idiomas RTL y modos de escritura verticales:

| Físico | Lógico |
|--------|--------|
| `padding-top`/`bottom` | `padding-block-start`/`end` |
| `padding-left`/`right` | `padding-inline-start`/`end` |
| `margin-left`/`right` | `margin-inline-start`/`end` |

`margin-inline: auto` (centrar) o `padding-block: 1rem` (relleno vertical) se adaptan a la dirección del texto. Ver [[02 direction | direction]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Padding para el espacio **interior** y el área clicable; margin para el espacio **entre** elementos.
> - Pon `box-sizing: border-box` para que `width` incluya padding y borde (más intuitivo).
> - Usa propiedades lógicas (`padding-inline`, `margin-block`) para soportar RTL.
> - El fondo cubre el padding: aprovéchalo para rellenos coloreados.

## Errores comunes

> [!warning] Trampas
> - **Margin para agrandar el área clicable**: no recibe eventos; usa padding.
> - **Esperar fondo en el margin**: siempre es transparente.
> - **Ignorar `box-sizing`**: el padding/borde se suman a `width` por defecto.

## Notas relacionadas

- [[03 Padding]] · [[05 Margin]] · [[04 Border/index]] — cada capa en detalle.
- [[06 box-sizing]] — cómo se suman al `width`.
- [[07 Margin Collapse]] — el colapso de márgenes verticales.
