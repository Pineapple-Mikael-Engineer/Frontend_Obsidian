---
title: Dimensión — width, height, aspect-ratio, orientation
aliases:
  - media query dimension
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 4
---

# Dimensión (width, height, aspect-ratio)

> [!definicion]
> Las features de **dimensión** consultan el tamaño del viewport: su ancho (`width`), alto (`height`), proporción (`aspect-ratio`) y orientación (`orientation`). El ancho es, con diferencia, la más usada para el diseño responsivo.

```css
@media (min-width: 768px) { }
@media (orientation: landscape) { }
@media (min-aspect-ratio: 16/9) { }
```

## Las features de tamaño

| Feature | Consulta | Prefijos |
|---------|----------|----------|
| `width` | Ancho del viewport | `min-width`, `max-width` |
| `height` | Alto del viewport | `min-height`, `max-height` |
| `aspect-ratio` | Proporción ancho/alto | `min-`, `max-` |
| `orientation` | `portrait` o `landscape` | — |

## width: la reina del responsive

El **ancho** es la base de casi todo breakpoint. `min-width` (mobile-first) y `max-width` (desktop-first) son las features más escritas en cualquier hoja responsiva:

```css
@media (min-width: 768px)  { /* tablet+ */ }
@media (min-width: 1024px) { /* escritorio */ }
```

## orientation: vertical u horizontal

`orientation` detecta si el dispositivo está en **vertical** (`portrait`, más alto que ancho) u **horizontal** (`landscape`):

```css
@media (orientation: landscape) {
  .galeria { grid-template-columns: repeat(4, 1fr); }   /* más columnas en horizontal */
}
```

> [!info] orientation se basa en la proporción, no en el dispositivo
> `landscape` significa que el viewport es **más ancho que alto**, no necesariamente "es un móvil girado". Una ventana de escritorio estrecha y alta es `portrait`. Por eso, para layout, `min-width` suele ser más fiable que `orientation` (que un móvil girado tenga 4 columnas no siempre es lo deseado).

## height: úsala con cuidado

> [!warning] Las media queries de altura son traicioneras
> `min-height`/`max-height` existen pero se usan poco y con cautela: la altura del viewport cambia con la barra del navegador, el teclado virtual y el scroll, lo que hace las queries de altura **inestables** en móvil. Casi siempre el responsive se basa en el ancho. Una excepción razonable: ajustar un hero en pantallas muy bajas (`@media (max-height: 500px)`).

## aspect-ratio: proporción del viewport

`aspect-ratio` consulta la proporción del viewport, útil para layouts que dependen de la forma de la pantalla (un juego, una presentación):

```css
@media (min-aspect-ratio: 16/9) {
  /* pantallas muy panorámicas */
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Basa el responsive en `min-width` (mobile-first); es lo más fiable.
> - Usa `orientation` con cuidado (se basa en la proporción, no en el tipo de dispositivo).
> - Evita las media queries de `height` salvo casos concretos (pantallas muy bajas).
> - Recuerda la sintaxis de rangos (`width >= 768px`) por legibilidad.

## Errores comunes

> [!warning] Trampas
> - **Basar el layout en `orientation`** esperando "móvil vs. escritorio": es proporción, no dispositivo.
> - **Media queries de altura** inestables por la barra/teclado del navegador.
> - **Off-by-one** entre `max-width` y `min-width` en el mismo punto.

## Notas relacionadas

- [[03 Operadores Lógicos]] — combinar dimensión con otras features.
- [[07 Breakpoints Comunes]] — los anchos típicos.
- [[03 aspect-ratio]] — la propiedad `aspect-ratio` (distinta de la media feature).
