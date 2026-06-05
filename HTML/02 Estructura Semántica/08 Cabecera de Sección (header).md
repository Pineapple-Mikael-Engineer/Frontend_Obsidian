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
---

# Cabecera de Sección (header)

> [!definicion]
> `<header>` agrupa el **contenido introductorio** de su contexto: logo y navegación si es de la
> página; título, autor y fecha si es de un [[06 Artículos (article) | `<article>`]]. No confundir
> con [[03 Cabecera (head)/index | `<head>`]] (metadatos invisibles): `<header>` es contenido
> visible.

```html
<header>
  <img src="logo.svg" alt="Mi sitio" />
  <nav>…</nav>
</header>
```

## Dos roles según el contexto

| Ubicación | Contenido típico | Landmark |
|-----------|------------------|----------|
| Hijo directo del `<body>` | Logo + navegación global | `banner` |
| Dentro de `<article>` / `<section>` | Título + metadatos del bloque | ninguno |

El mismo elemento cambia de significado según dónde esté: como banner de página solo debe haber
uno; como cabecera de artículo puede haber muchos.

> [!info] Banner solo en el nivel superior
> `<header>` se expone como landmark `banner` **únicamente** cuando es hijo directo del `<body>` (no
> anidado en otro elemento seccionador). Dentro de un `<article>`, es solo una cabecera local sin
> rol de landmark.

> [!warning] header ≠ head
> Error frecuente de principiante: `<head>` (singular, metadatos en el documento) y `<header>`
> (contenido visible) son elementos distintos. `<head>` va una vez arriba; `<header>` va dentro del
> `<body>`.

## Notas relacionadas

- [[09 Pie de Sección (footer)]] — su contraparte al cierre.
- [[03 Cabecera (head)/index]] — el `<head>` de metadatos, no confundir.
