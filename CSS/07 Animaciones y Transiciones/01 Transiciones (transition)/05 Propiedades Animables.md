---
title: Propiedades Animables — Qué se puede transicionar
aliases:
  - animatable properties
tags:
  - css
  - api/concepto
  - animacion
draft: false
---

# Propiedades Animables

> [!definicion]
> No todas las propiedades CSS se pueden animar. Una propiedad es **animable** si sus valores se pueden **interpolar** (calcular puntos intermedios): un color va de rojo a azul pasando por tonos intermedios, pero `display` salta de `block` a `none` sin estados intermedios. Conocer qué se puede animar evita transiciones que "no funcionan".

```css
/* ✅ animable: hay valores intermedios */
.x { transition: opacity 0.3s, transform 0.3s, background 0.3s; }
/* ❌ no animable (clásicamente): cambio discreto */
.x { transition: display 0.3s; }   /* no hace nada */
```

## Qué se puede y qué no

| Animable (interpolable) | No animable (discreto) |
|-------------------------|------------------------|
| `opacity`, `color`, `background-color` | `display` |
| `transform`, `width`, `height` | `visibility` (solo on/off, sin intermedios) |
| `margin`, `padding`, `border-width` | `font-family` |
| `box-shadow`, `border-radius` | `position` |
| `top`/`left`, `font-size`, `line-height` | `flex-direction`, `text-align` |
| `filter`, `gap` | `cursor`, `z-index` (salta) |

> [!info] La regla: ¿hay un punto intermedio?
> Una propiedad es animable si tiene sentido un valor "a mitad de camino". Entre `width: 100px` y `width: 200px` está `150px` (animable). Entre `display: block` y `display: none` no hay "medio bloque" (discreto). El color, las longitudes, los `transform` y la opacidad son interpolables; las palabras clave estructurales, no.

## El problema clásico: animar a/desde auto

> [!warning] No se puede animar height: auto
> Una limitación famosa: **`height: auto` (y `width: auto`) no se pueden animar**. El navegador no conoce el valor final en píxeles de `auto` hasta que renderiza, así que no puede interpolar. Animar de `0` a `auto` **no funciona** (el cambio es instantáneo). Soluciones:
> - Animar `max-height` de `0` a un valor grande (impreciso pero funciona).
> - Usar `transform: scaleY()` (eficiente, pero deforma el contenido).
> - Las propiedades modernas `interpolate-size: allow-keywords` y `calc-size()` empiezan a permitir animar a `auto` (soporte creciente).

## Animar display (moderno)

> [!info] @starting-style y allow-discrete
> `display` no se animaba clásicamente, pero CSS moderno lo permite con `transition-behavior: allow-discrete` y `@starting-style` (que define el estado inicial de un elemento que aparece). Esto habilita transiciones de entrada/salida con `display: none` sin JavaScript, aunque el soporte aún crece:
> ```css
> .panel {
>   transition: opacity 0.3s, display 0.3s allow-discrete;
> }
> @starting-style { .panel.abierto { opacity: 0; } }
> ```

## La preferencia por transform y opacity

> [!tip] Animables Y eficientes
> Aunque muchas propiedades sean animables, no todas son **eficientes**. `transform` y `opacity` son animables **y** baratas (las gestiona la GPU sin recalcular el layout). Animar `width`/`margin`/`top` es animable pero **caro** (provoca reflow). Por rendimiento, se prefiere `transform` aunque a veces haya que repensar el efecto. Detalle en [[03 Animar transform y opacity]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Comprueba que la propiedad sea animable antes de transicionarla.
> - Para entrada/salida con cambio de tamaño, usa `transform` o `max-height`, no `height: auto`.
> - Prefiere `transform`/`opacity` por rendimiento, no solo por ser animables.
> - Para animar `display`, usa `allow-discrete` + `@starting-style` (soporte moderno).

## Errores comunes

> [!warning] Trampas
> - **Transicionar `display`/`visibility`** esperando un fundido clásico.
> - **Animar `height: auto`**: no funciona; usa alternativas.
> - **Animar propiedades caras** (layout) cuando `transform` daría el mismo efecto.

## Notas relacionadas

- [[01 transition-property]] — declarar qué animar.
- [[03 Animar transform y opacity]] — las propiedades animables eficientes.
- [[04 display none]] — `display`, el caso de animación discreta.
