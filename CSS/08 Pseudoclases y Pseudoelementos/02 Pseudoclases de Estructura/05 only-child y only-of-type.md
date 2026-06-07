---
title: only-child y only-of-type — Hijo único
aliases:
  - ":only-child"
  - ":only-of-type"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":only-child"
grupo: pseudoclase
draft: false
---

# :only-child y :only-of-type

> [!definicion]
> `:only-child` selecciona un elemento si es el **único hijo** de su padre (no tiene hermanos); `:only-of-type`, si es el **único de su tipo** entre sus hermanos. Permiten dar un estilo distinto cuando un elemento está "solo".

```css
li:only-child { list-style: none; }   /* una lista de un solo elemento */
```

## only-child: sin hermanos

`:only-child` coincide cuando el elemento es el **único** hijo. Equivale a ser `:first-child` **y** `:last-child` a la vez:

```css
/* Una lista con un solo item: quitar la viñeta y centrar */
ul li:only-child { list-style: none; text-align: center; }

/* Un párrafo solo en su contenedor: darle más espacio */
.card p:only-child { padding: 2rem; }
```

## only-of-type: único de su tipo

`:only-of-type` coincide cuando no hay **otros del mismo tipo**, aunque haya hermanos de otros tipos:

```html
<div>
  <h2>Título</h2>
  <img>   <!-- :only-of-type SÍ (es la única <img>) -->
</div>
```

```css
/* La única imagen de un bloque ocupa todo el ancho */
.contenido img:only-of-type { width: 100%; }
```

## Casos de uso

> [!tip] Adaptar el diseño a la cantidad
> Estas pseudoclases permiten que un componente se vea distinto según tenga **uno** o **varios** elementos, sin JavaScript:
> ```css
> /* Si hay un solo botón, que ocupe todo el ancho; si hay varios, que se repartan */
> .acciones button:only-child { width: 100%; }
>
> /* Un avatar solo vs. un grupo de avatares */
> .avatares img:only-child { width: 4rem; }
> ```
> Es una forma elegante de hacer layouts que reaccionan al número de hijos.

## La relación con :empty

`:only-child` mira que **no haya hermanos**; [[06 empty | `:empty`]] mira que el elemento **no tenga contenido**. Son conceptos distintos: un hijo único puede tener mucho contenido, y un elemento vacío puede tener hermanos.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:only-child` para adaptar el estilo cuando hay un solo elemento (botón a ancho completo, item sin viñeta).
> - `:only-of-type` cuando el elemento es único de su tipo pero hay otros de tipos distintos.
> - Permiten layouts que reaccionan a la cantidad sin JS.
> - No los confundas con `:empty` (sin hermanos vs. sin contenido).

## Errores comunes

> [!warning] Trampas
> - **Confundir `:only-child` con `:empty`**: uno es sin hermanos, otro sin contenido.
> - **Esperar que `:only-of-type` ignore** los hermanos de otros tipos: sí los ignora (es lo correcto).

## Notas relacionadas

- [[01 first-child y last-child]] — `:only-child` = ambos a la vez.
- [[06 empty]] — el elemento sin contenido (distinto).
- [[03 has()]] — adaptar según el contenido de forma más general.
