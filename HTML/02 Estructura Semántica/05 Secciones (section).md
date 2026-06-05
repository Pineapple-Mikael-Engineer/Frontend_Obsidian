---
title: <section> — Sección temática con su propio título
aliases:
  - section element
tags:
  - html
  - api/elemento
  - semantica
elemento: section
categoria: seccionado
rol_implicito: region
vacio: false
draft: false
---

# Secciones (section)

> [!definicion]
> `<section>` agrupa contenido **temáticamente relacionado** que forma una parte del documento y que
> normalmente **lleva su propio encabezado**. Es una división del contenido, como un capítulo o un
> apartado.

```html
<section>
  <h2>Preguntas frecuentes</h2>
  <p>…</p>
</section>
<section>
  <h2>Testimonios</h2>
  <p>…</p>
</section>
```

## section vs article vs div

| Elemento | Cuándo |
|----------|--------|
| `<section>` | Parte temática del documento, con título; **no** tiene sentido por sí sola fuera |
| `<article>` | Contenido **autónomo** que se podría sindicar o reutilizar aislado |
| `<div>` | Sin valor semántico; solo agrupar para estilo o script |

La prueba: si el bloque tendría sentido publicado por separado (un post, una tarjeta de producto),
es [[06 Artículos (article) | `<article>`]]; si es un apartado del todo (la sección "Contacto" de
una landing), es `<section>`.

> [!warning] Casi siempre necesita encabezado
> Una `<section>` sin un `<h2>`–`<h6>` que la titule suele ser señal de que debía ser un `<div>`. El
> esquema del documento espera que cada sección aporte un título.

> [!info] Landmark "region" condicional
> `<section>` solo se anuncia como landmark a los lectores de pantalla si tiene nombre accesible
> (`aria-label` o un encabezado referenciado con `aria-labelledby`). Sin nombre, pasa
> desapercibida.

## Notas relacionadas

- [[06 Artículos (article)]] — la alternativa para contenido autónomo.
- [[01 Encabezados Jerárquicos (h1-h6)]] — el título que toda sección espera.
