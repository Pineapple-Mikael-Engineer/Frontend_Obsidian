---
title: Decoración de Texto Avanzada — Sombras, énfasis, trazo
aliases:
  - advanced text decoration
tags:
  - css
  - api/concepto
  - tipografia
draft: false
---

# Decoración de Texto Avanzada

> [!definicion]
> Más allá de [[09 text-decoration | `text-decoration`]] (subrayados y tachados), CSS ofrece efectos visuales para el texto: **sombras** (`text-shadow`), **marcas de énfasis** orientales (`text-emphasis`), **trazo/contorno** (`text-stroke`) y el **ajuste fino del subrayado** (`text-underline-offset`/`-position`).

```css
h1 { text-shadow: 0 2px 4px rgb(0 0 0 / 0.3); }
.contorno { -webkit-text-stroke: 1px #cba6f7; }
```

## Mapa de la subsección

- [[01 text-shadow]] — sombras del texto (legibilidad, profundidad, brillo).
- [[02 text-emphasis]] — marcas de énfasis sobre/bajo cada carácter (CJK).
- [[03 text-stroke]] — trazo o contorno de las letras.
- [[04 text-underline-offset y position]] — afinar la posición del subrayado.

## Efectos vs. legibilidad

> [!warning] La decoración no debe estorbar la lectura
> Estos efectos son potentes pero pueden **dañar la legibilidad** si se abusa: una `text-shadow` muy marcada emborrona el texto, un `text-stroke` grueso lo hace ilegible. Su mejor uso es **funcional** (una sombra sutil que mejora el contraste de texto sobre una imagen) más que puramente decorativo. El texto, ante todo, debe leerse.

## Notas relacionadas

- [[01 text-shadow]] — el más usado, sobre todo para contraste.
- [[09 text-decoration]] — la decoración básica (subrayado).
- [[03 Sombras (box-shadow)]] — sombras de cajas, distinto de `text-shadow`.
