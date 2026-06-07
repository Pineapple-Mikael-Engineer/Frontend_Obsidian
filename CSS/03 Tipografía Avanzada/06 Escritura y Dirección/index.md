---
title: Escritura y Dirección — Modos de escritura e internacionalización
aliases:
  - writing modes
  - text direction
tags:
  - css
  - api/concepto
  - tipografia
draft: false
---

# Escritura y Dirección

> [!definicion]
> CSS soporta sistemas de escritura distintos del horizontal-izquierda-a-derecha occidental: texto **vertical** (japonés tradicional, chino), de **derecha a izquierda** (árabe, hebreo) y sus combinaciones. Estas propiedades controlan la **dirección** y **orientación** del flujo de texto para cualquier idioma.

```css
.vertical { writing-mode: vertical-rl; }   /* texto vertical, de derecha a izquierda */
.arabe    { direction: rtl; }              /* de derecha a izquierda */
```

## Mapa de la subsección

- [[01 writing-mode]] — horizontal o vertical, y en qué sentido fluyen las líneas.
- [[02 direction]] — la dirección del texto en línea (LTR/RTL).
- [[03 text-orientation]] — cómo se orientan los glifos en modo vertical.
- [[04 unicode-bidi]] — el control del algoritmo bidireccional.

## Por qué importa: la web es global

> [!info] No todo se escribe como el español
> Aunque el desarrollo occidental asume horizontal-LTR, la web sirve a idiomas con otras direcciones: el árabe y el hebreo van de **derecha a izquierda**; el japonés y el chino tradicionales pueden ir en **columnas verticales** de derecha a izquierda. Estas propiedades, junto con las **propiedades lógicas** (`margin-inline`, `padding-block`…), permiten un mismo CSS que se adapta a cualquier dirección sin reescribirlo.

## Físico vs. lógico

> [!tip] Piensa en inline/block, no en left/right
> La clave de un CSS internacionalizable es razonar en ejes **lógicos** (inline = dirección del texto, block = dirección de las líneas) en vez de físicos (left/right/top/bottom):
> | Físico | Lógico |
> |--------|--------|
> | `width` | `inline-size` |
> | `height` | `block-size` |
> | `margin-left` | `margin-inline-start` |
> | `top` | `inset-block-start` |
>
> Con propiedades lógicas, cambiar `direction` o `writing-mode` reorienta todo el layout automáticamente.

## Notas relacionadas

- [[02 direction]] — el caso más común (RTL).
- [[01 writing-mode]] — la base de la escritura vertical.
- [[04 Idioma y Dirección (lang, dir)]] — el atributo `dir` desde HTML.
