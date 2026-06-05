---
title: <article> — Contenido autónomo y redistribuible
aliases:
  - article element
tags:
  - html
  - api/elemento
  - semantica
elemento: article
categoria: seccionado
rol_implicito: article
vacio: false
draft: false
---

# Artículos (article)

> [!definicion]
> `<article>` representa una unidad de contenido **autónoma**: tendría sentido por sí sola, extraída
> de la página, en un lector RSS o compartida aparte. Un post de blog, una noticia, un comentario,
> una tarjeta de producto.

```html
<article>
  <header>
    <h2>Cómo regar un cactus</h2>
    <p>Publicado el <time datetime="2026-06-04">4 jun 2026</time></p>
  </header>
  <p>Contenido del artículo…</p>
</article>
```

## La prueba de autonomía

Si el bloque, copiado a otro sitio sin contexto, **sigue teniendo sentido completo**, es un
`<article>`. Si depende del resto de la página para entenderse, es una
[[05 Secciones (section) | `<section>`]].

Casos típicos: post de blog · noticia · comentario de usuario · ficha de producto · widget
independiente.

## Anidamiento

Un `<article>` puede contener otros `<article>`: un post (artículo) con sus comentarios (cada uno un
artículo anidado). Cada uno suele tener su propio [[08 Cabecera de Sección (header) | `<header>`]] y
[[09 Pie de Sección (footer) | `<footer>`]].

> [!info] Reutilización semántica
> El valor de `<article>` se nota en agregadores y lectores: marca exactamente la unidad
> sindicable. Para SEO y datos estructurados, suele acompañarse de JSON-LD `Article`.

> [!tip] section dentro de article y viceversa
> Un `<article>` largo puede dividirse en `<section>` internas (apartados del post). Y una
> `<section>` de la página puede contener varios `<article>` (la sección "Últimas noticias" con
> varias noticias). Ambos anidamientos son válidos: la pregunta siempre es *¿autónomo o parte?*.

## Notas relacionadas

- [[05 Secciones (section)]] — la división temática no autónoma.
- [[23 Tiempo (time)]] — para marcar la fecha de publicación.
