---
title: Dimensiones — Controlar el tamaño de las cajas
aliases:
  - sizing
  - dimensiones
tags:
  - css
  - api/concepto
  - layout
draft: false
order: 1
---

# Dimensiones

> [!definicion]
> Las propiedades de **dimensión** controlan el tamaño de una caja: `width`/`height` fijan un tamaño, `min`/`max` lo acotan entre límites, `aspect-ratio` mantiene una proporción, y las palabras `min-content`/`max-content`/`fit-content` dimensionan según el contenido. Juntas permiten cajas fijas, fluidas y adaptables.

```css
.caja {
  width: 100%;
  max-width: 60rem;        /* fluida pero con tope */
  aspect-ratio: 16 / 9;    /* proporción fija */
}
```

## Mapa de la subsección

- [[01 width y height]] — el tamaño base de la caja.
- [[02 min y max (width, height)]] — acotar entre un mínimo y un máximo.
- [[03 aspect-ratio]] — mantener una proporción sin trucos.
- [[04 Tamaños de Contenido (fit-content, min-content, max-content)]] — dimensionar por el contenido.

## El patrón fluido con tope

> [!tip] width + max-width, la receta de oro
> La combinación más útil de dimensiones: un ancho **fluido** (`width: 100%` o `%`) con un **tope** (`max-width` en `rem`). El elemento ocupa lo disponible en pantallas pequeñas pero no se estira sin límite en grandes:
> ```css
> .contenedor { width: 100%; max-width: 65rem; margin-inline: auto; }
> ```
> Es la base de casi todo contenedor centrado responsivo.

## width afecta al contenido (con content-box)

Recuerda que, con el `box-sizing` por defecto ([[06 box-sizing | content-box]]), `width`/`height` miden **solo el contenido**: padding y borde se suman aparte. Con `border-box`, lo incluyen. Esto cambia por completo cómo se interpretan las dimensiones.

## Bloques vs. en línea

> [!info] Las dimensiones no aplican igual a todo
> `width`/`height` funcionan en elementos de **bloque** y `inline-block`, pero **no** en elementos puramente **en línea** (`<span>`, `<a>` sin `display` cambiado): un elemento inline toma el tamaño de su contenido y ignora `width`/`height`. Para dimensionar un inline, hay que cambiar su [[03 display inline-block | `display`]]. Detalle en [[01 Flujo Normal del Documento/index | flujo normal]].

## Notas relacionadas

- [[01 width y height]] — el punto de partida.
- [[06 box-sizing]] — qué incluyen las dimensiones.
- [[05 Porcentajes]] — los `%` en `width`/`height`.
