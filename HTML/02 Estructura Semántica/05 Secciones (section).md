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
order: 5
---

# Secciones (section)

> [!definicion]
> `<section>` agrupa contenido **temáticamente relacionado** que forma una parte del documento y que
> normalmente **lleva su propio encabezado**. Es una división del contenido —un capítulo, un
> apartado— que tiene sentido como parte del todo, pero no necesariamente aislada.

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

## section vs. article vs. div

Es la decisión que más dudas genera. El criterio:

| Elemento | Cuándo usarlo |
|----------|---------------|
| `<section>` | Parte temática del documento, con título; no es autónoma fuera de contexto |
| `<article>` | Contenido **autónomo**: tendría sentido publicado o sindicado por separado |
| `<div>` | Sin valor semántico; solo agrupar para estilo o script |

La prueba decisiva: *si extraigo este bloque y lo publico solo, ¿se entiende completo?* Si sí, es
[[06 Artículos (article) | `<article>`]] (un post, una ficha de producto). Si es un apartado del
conjunto (la sección "Contacto" de una landing), es `<section>`. Si no aporta significado y solo
agrupo para aplicar CSS, es `<div>`.

## Casi siempre necesita encabezado

Una `<section>` es, por definición, una sección **de algo**, y las secciones tienen título. Una
`<section>` sin un `<h2>`–`<h6>` que la encabece suele ser señal de que debía ser un `<div>`:

```html
<!-- ✅ Sección con título -->
<section>
  <h2>Envíos y devoluciones</h2>
  …
</section>

<!-- ❓ Sin título: ¿de verdad es una sección, o un div de layout? -->
<section class="grid">…</section>
```

## section dentro de article y viceversa

Ambos anidamientos son válidos; la pregunta siempre es *¿autónomo o parte?*:

- Una `<section>` de la página ("Últimas noticias") puede contener varios `<article>` (cada noticia).
- Un `<article>` largo (un post extenso) puede dividirse en `<section>` internas (sus apartados).

## Accesibilidad

> [!info] Landmark region solo con nombre
> `<section>` se expone como landmark "region" a las tecnologías de asistencia **únicamente si tiene
> un nombre accesible**: un `aria-label` o un encabezado referenciado con `aria-labelledby`. Sin
> nombre, no aparece en el mapa de landmarks (para no inundarlo de regiones anónimas).
> ```html
> <section aria-labelledby="faq-titulo">
>   <h2 id="faq-titulo">Preguntas frecuentes</h2>
> </section>
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<section>` cuando el bloque sea un apartado temático **con título**.
> - Si vas a apoyarte en su rol de landmark, dale nombre con `aria-labelledby` apuntando a su `<h2>`.
> - Ante la duda entre `<section>` y `<div>`: si no hay título ni tema propio, `<div>`.

## Errores comunes

> [!warning] section como div decorativo
> Usar `<section>` para cualquier contenedor "porque suena más semántico" es un antipatrón: una
> sección sin tema ni título no aporta significado y ensucia el esquema. El contenedor neutro para
> maquetar es `<div>`. La semántica no se gana cambiando `<div>` por `<section>` a ciegas.

## Notas relacionadas

- [[06 Artículos (article)]] — la alternativa para contenido autónomo.
- [[01 Encabezados Jerárquicos (h1-h6)]] — el título que toda sección espera.
