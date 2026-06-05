---
title: <main> — Contenido principal del documento
aliases:
  - main element
tags:
  - html
  - api/elemento
  - semantica
elemento: main
categoria: flujo
rol_implicito: main
vacio: false
draft: false
---

# Contenido Principal (main)

> [!definicion]
> `<main>` envuelve el **contenido central y único** de la página: lo que la distingue del resto del
> sitio. Excluye lo que se repite en todas las páginas (cabecera, navegación, pie). Debe haber
> **uno solo** visible por documento.

```html
<body>
  <header>…</header>
  <nav>…</nav>
  <main>
    <h1>Artículo del día</h1>
    <article>…</article>
  </main>
  <footer>…</footer>
</body>
```

## Reglas

| Regla | Razón |
|-------|-------|
| Uno solo por página | El "contenido principal" es único por definición |
| No anidar en `<article>`, `<aside>`, `<header>`, `<footer>`, `<nav>` | `<main>` es de nivel superior |
| No incluir contenido repetido entre páginas | Logo, menú y pie van fuera |

> [!info] El destino del "saltar al contenido"
> `<main>` expone el landmark "main". Los lectores de pantalla ofrecen una tecla rápida para saltar
> directamente a él, y los [[03 Skip Links | skip links]] suelen apuntar aquí (`<a href="#main">`).
> Es el ancla de accesibilidad más útil de la página.

> [!warning] No es un contenedor de estilo
> `<main>` no debe elegirse por conveniencia de maquetación. Su valor es semántico: "aquí empieza lo
> importante". Para agrupar por estilo se usa un [[01 Identificación (id, class) | `<div>`]].

## Notas relacionadas

- [[04 Cuerpo (body)]] — `<main>` vive directamente bajo el `<body>`.
- [[03 Skip Links]] — el enlace de salto que apunta al `<main>`.
- [[06 Artículos (article)]] — el contenido autónomo que suele ir dentro.
