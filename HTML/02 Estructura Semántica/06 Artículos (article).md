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
> de la página, en un lector de feeds o compartida aparte. Un post de blog, una noticia, un
> comentario, una tarjeta de producto, un widget independiente.

```html
<article>
  <header>
    <h2>Cómo regar un cactus</h2>
    <p>Publicado el <time datetime="2026-06-04">4 jun 2026</time></p>
  </header>
  <p>Contenido del artículo…</p>
  <footer>
    <p>Etiquetas: jardinería, suculentas</p>
  </footer>
</article>
```

## La prueba de autonomía

El criterio único para decidir `<article>` vs. [[05 Secciones (section) | `<section>`]]:

> Si copio este bloque a otro sitio **sin nada de contexto** y sigue teniendo sentido completo, es un
> `<article>`. Si depende del resto de la página para entenderse, es una `<section>`.

Casos típicos de `<article>`:

- Post de blog o noticia.
- Comentario de usuario.
- Ficha de producto en un listado.
- Tarjeta de una red social.
- Widget reutilizable (un "tiempo de hoy").

## Cada article puede tener su header y footer

Como unidad completa, un `<article>` suele incluir sus propios
[[08 Cabecera de Sección (header) | `<header>`]] (título, autor, fecha) y
[[09 Pie de Sección (footer) | `<footer>`]] (etiquetas, metadatos), distintos de los de la página:

```html
<article>
  <header><h2>Título</h2><p>Por Ana · <time datetime="2026-06-04">ayer</time></p></header>
  <p>…</p>
  <footer><p>3 comentarios</p></footer>
</article>
```

## Artículos anidados

Un `<article>` puede contener otros `<article>`: el caso clásico es un post (artículo) con sus
comentarios (cada comentario, un artículo anidado, porque cada uno es una unidad autónoma):

```html
<article> <!-- el post -->
  <h2>Mi viaje a Japón</h2>
  <p>…</p>
  <section> <!-- zona de comentarios -->
    <h3>Comentarios</h3>
    <article><p>¡Qué fotos tan bonitas!</p></article>
    <article><p>Gracias por los consejos.</p></article>
  </section>
</article>
```

## SEO y datos estructurados

> [!info] article + JSON-LD
> `<article>` marca la unidad sindicable para agregadores y lectores de feeds. Para que los buscadores
> la traten como artículo enriquecido (con autor, fecha y titular en los resultados), se acompaña de
> datos estructurados JSON-LD con el tipo `Article`. El elemento HTML da la estructura; el JSON-LD da
> los metadatos explícitos.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<article>` para cada unidad repetible de un listado (cada post, cada producto).
> - Dale a cada uno su `<header>` con título y, si aplica, su fecha con
>   [[23 Tiempo (time) | `<time>`]].
> - Para contenido que es un apartado del todo (no autónomo), usa `<section>`.

## Errores comunes

> [!warning] Confundir article con section
> El error frecuente es usarlos como sinónimos. Un apartado de una página (sin sentido fuera) no es
> `<article>`; una unidad autónoma repetida no es `<section>`. Ante la duda, aplica la prueba de
> autonomía. Y recuerda: ninguno es un `<div>` con mejor nombre; ambos comunican significado real.

## Notas relacionadas

- [[05 Secciones (section)]] — la división temática no autónoma.
- [[23 Tiempo (time)]] — para marcar la fecha de publicación del artículo.
- [[08 Cabecera de Sección (header)]] — la cabecera propia de cada artículo.
