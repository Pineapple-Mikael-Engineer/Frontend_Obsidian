---
title: Flujo Normal del Documento — Cómo se colocan las cajas por defecto
aliases:
  - normal flow
  - display
tags:
  - css
  - api/concepto
  - layout
draft: false
order: 1
---

# Flujo Normal del Documento

> [!definicion]
> El **flujo normal** es cómo el navegador coloca las cajas **sin** ningún layout especial: los elementos de **bloque** se apilan verticalmente (uno bajo otro) y los elementos **en línea** fluyen horizontalmente (uno tras otro, como las palabras). La propiedad `display` define a qué categoría pertenece cada elemento.

```css
div  { display: block; }         /* ocupa toda la línea, apila */
span { display: inline; }        /* fluye con el texto */
.x   { display: inline-block; }  /* en línea pero con dimensiones */
.y   { display: none; }          /* fuera del flujo y de la vista */
```

## Los valores de display de esta sección

| Valor | Comportamiento |
|-------|----------------|
| `block` | Ocupa todo el ancho, apila verticalmente |
| `inline` | Fluye con el texto, sin dimensiones |
| `inline-block` | En línea pero acepta `width`/`height` |
| `none` | No se renderiza (fuera del flujo) |

Cada uno en su nota: [[01 display block]], [[02 display inline]], [[03 display inline-block]] y [[04 display none]].

(Los valores `flex` y `grid` activan sus propios sistemas, en [[03 Flexbox/index]] y [[04 CSS Grid/index]].)

## Bloque vs. en línea: la distinción base

> [!info] Apilar vs. fluir
> Es la distinción más fundamental del layout:
> - Un elemento **de bloque** (`<div>`, `<p>`, `<h1>`, `<section>`) empieza en una **línea nueva**, ocupa todo el ancho disponible y respeta `width`/`height`. Se **apilan** verticalmente.
> - Un elemento **en línea** (`<span>`, `<a>`, `<strong>`, `<em>`) fluye **dentro** del texto, solo ocupa lo que necesita su contenido, e **ignora** `width`/`height`. Fluyen horizontalmente.
>
> Cada elemento HTML tiene un `display` por defecto (un `<p>` es block, un `<a>` es inline), que CSS puede cambiar.

## display es también la puerta a Flexbox y Grid

`display: flex` y `display: grid` no solo cambian la categoría del elemento: convierten su **contenido** en un contexto de layout flexible o de rejilla. Por eso `display` es la propiedad central del layout: define tanto el comportamiento del elemento como el de sus hijos.

## Notas relacionadas

- [[01 display block]] · [[02 display inline]] — la distinción fundamental.
- [[03 display inline-block]] — el híbrido.
- [[01 Contenedor Flex (display flex)]] — `display: flex`, el siguiente nivel.
