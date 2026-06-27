---
title: text-underline-offset y position — Afinar el subrayado
aliases:
  - text-underline-offset
  - text-underline-position
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: text-underline-offset
grupo: tipografia
valor_inicial: auto
hereda: true
animable: true
draft: false
order: 4
---

# text-underline-offset y position

> [!definicion]
> Dos propiedades afinan el aspecto del subrayado de [[09 text-decoration | `text-decoration`]]: `text-underline-offset` controla la **distancia** entre el subrayado y el texto, y `text-underline-position` su **posición** general (bajo la línea base o bajo los descendentes). Permiten subrayados elegantes que no chocan con las letras.

```css
a {
  text-decoration-line: underline;
  text-underline-offset: 0.2em;   /* separa la línea del texto */
}
```

## text-underline-offset: la separación

| Valor | Efecto |
|-------|--------|
| `auto` | El navegador decide (por defecto) |
| Longitud | `2px`, `0.2em`: separa el subrayado del texto |

> [!tip] Un poco de offset mejora mucho el subrayado
> Por defecto, el subrayado va pegado a las letras y a veces las **toca** (sobre todo en los descendentes: "p", "g", "y"). Un pequeño `text-underline-offset` lo separa, dándole un aspecto mucho más limpio y legible:
> ```css
> a {
>   text-underline-offset: 0.15em;
>   text-decoration-thickness: 0.08em;
> }
> ```
> Junto con `text-decoration-skip-ink` (que hace que la línea salte los descendentes), produce subrayados de calidad tipográfica.

## text-underline-position: dónde va

| Valor | Posición del subrayado |
|-------|------------------------|
| `auto` | Posición por defecto (cerca de la línea base) |
| `under` | **Bajo** los descendentes (no los cruza) |
| `from-font` | Donde la fuente indique (si lo define) |
| `left` / `right` | Para escritura vertical |

`under` es útil para que el subrayado nunca cruce las colas de las letras, importante en notación matemática o científica donde el subrayado no debe tocar el texto.

## em para que escale

> [!tip] Usa em en el offset
> Como el subrayado debe mantener una proporción con el tamaño de la letra, `text-underline-offset` y `text-decoration-thickness` se expresan mejor en **`em`**: así un título grande y un texto pequeño tienen subrayados proporcionados.

## Animar el subrayado

Como `text-underline-offset` es animable, se pueden hacer subrayados que crecen o se separan al pasar el ratón, aunque para subrayados animados elaborados se suele usar un `background` con gradiente:

```css
a { text-underline-offset: 0.15em; transition: text-underline-offset 0.2s; }
a:hover { text-underline-offset: 0.3em; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Añade un pequeño `text-underline-offset` (en `em`) para que el subrayado no toque las letras.
> - Combínalo con `text-decoration-thickness` y `skip-ink` para subrayados elegantes.
> - `text-underline-position: under` cuando el subrayado no deba cruzar los descendentes.
> - Mantén el subrayado en los enlaces (no lo quites): solo afínalo.

## Errores comunes

> [!warning] Trampas
> - **Subrayado pegado** que cruza los descendentes: feo; usa offset.
> - **Offset en `px`**: no escala con el tamaño de la fuente.
> - **Quitar el subrayado** de enlaces en vez de afinarlo (accesibilidad).

## Notas relacionadas

- [[09 text-decoration]] — la propiedad base del subrayado.
- [[09 Subrayado (u)]] — el subrayado desde HTML.
- [[01 Enlaces (a)]] — los enlaces, principales beneficiarios.
