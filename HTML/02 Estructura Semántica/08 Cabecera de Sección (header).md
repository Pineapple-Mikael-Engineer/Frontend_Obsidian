---
title: <header> — Cabecera de página o de sección
aliases:
  - header element
tags:
  - html
  - api/elemento
  - semantica
elemento: header
categoria: flujo
rol_implicito: banner
vacio: false
draft: false
order: 8
---

# Cabecera de Sección (header)

> [!definicion]
> `<header>` agrupa el **contenido introductorio** de su contexto. Como cabecera de página: logo,
> título del sitio y navegación principal. Como cabecera de un `<article>` o `<section>`: el título de
> ese bloque, autor y fecha. No confundir con `<head>` (metadatos invisibles del documento): `<header>`
> es contenido **visible** del `<body>`.

```html
<header>
  <a href="/" class="logo">Mi Sitio</a>
  <nav aria-label="Principal">…</nav>
</header>
```

## Dos roles según el contexto

El mismo elemento cambia de significado —y de rol de accesibilidad— según dónde esté:

| Ubicación | Contenido típico | ¿Landmark? |
|-----------|------------------|------------|
| Hijo directo del `<body>` | Logo, título del sitio, navegación global | Sí, `banner` |
| Dentro de `<article>` / `<section>` | Título del bloque, autor, fecha | No, cabecera local |

```html
<!-- Cabecera de página: banner -->
<body>
  <header> <img src="logo.svg" alt="Mi Sitio" /> <nav>…</nav> </header>
  …
</body>

<!-- Cabecera de artículo: local, sin rol de landmark -->
<article>
  <header> <h2>Título del post</h2> <p>Por Ana</p> </header>
  …
</article>
```

## Qué puede contener

`<header>` admite casi cualquier contenido de flujo: encabezados, navegación, logo, buscador,
eslogan. Lo que **no** puede contener es otro `<header>` ni un `<footer>` anidados directamente (no se
anidan entre sí).

## Accesibilidad

> [!info] Banner solo en el nivel superior
> `<header>` se expone como landmark "banner" **únicamente** cuando es hijo directo del `<body>` (no
> anidado dentro de `<article>`, `<section>`, `<main>`…). Solo debería haber un banner por página. Las
> cabeceras de artículo son locales y no generan landmark, lo cual es correcto: no quieres veinte
> "banner" en una lista de posts.

## Buenas prácticas

> [!tip] Recomendaciones
> - Un `<header>` de página (banner) por documento, como hijo directo del `<body>`.
> - Dentro, la navegación va en su propio [[03 Navegación (nav) | `<nav>`]].
> - Reutiliza `<header>` en cada `<article>` para su título y metadatos: es correcto y útil.

## Errores comunes

> [!warning] header ≠ head
> El error de principiante por excelencia: `<head>` (singular, metadatos del documento en el
> `<head>`) y `<header>` (contenido visible dentro del `<body>`) son elementos distintos. `<head>` va
> una vez, arriba del documento; `<header>` va dentro del `<body>` y puede repetirse. Otro error:
> meter varios `<header>` banner (varios hijos directos del body con rol banner) confunde a la
> asistencia.

## Notas relacionadas

- [[09 Pie de Sección (footer)]] — su contraparte de cierre.
- [[03 Cabecera (head)/index]] — el `<head>` de metadatos, que no debe confundirse con `<header>`.
- [[03 Navegación (nav)]] — la navegación que suele ir dentro del header de página.
