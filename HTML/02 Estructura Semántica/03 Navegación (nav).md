---
title: <nav> — Bloque de navegación
aliases:
  - nav element
  - navigation
tags:
  - html
  - api/elemento
  - semantica
elemento: nav
categoria: seccionado
rol_implicito: navigation
vacio: false
draft: false
---

# Navegación (nav)

> [!definicion]
> `<nav>` marca un bloque de **enlaces de navegación significativos**: el menú principal del sitio, la
> tabla de contenidos, la paginación, las migas de pan (breadcrumbs). Expone un landmark "navigation"
> que las tecnologías de asistencia listan y al que permiten saltar.

```html
<nav aria-label="Principal">
  <ul>
    <li><a href="/">Inicio</a></li>
    <li><a href="/blog">Blog</a></li>
    <li><a href="/contacto">Contacto</a></li>
  </ul>
</nav>
```

## Cuándo es nav y cuándo no

No todo grupo de enlaces merece `<nav>`: se reserva para conjuntos de navegación **primarios o
relevantes**.

| Sí es `<nav>` | No es `<nav>` |
|---------------|---------------|
| Menú principal del sitio | Un enlace suelto en un párrafo |
| Tabla de contenidos de un artículo | Enlaces de referencia dentro del texto |
| Paginación (1 2 3 →) | El enlace del logo a la home (basta el `<a>`) |
| Breadcrumbs | El listado de enlaces legales del pie (basta el `<footer>`) |

El pie de página suele tener enlaces (privacidad, términos): no es obligatorio envolverlos en `<nav>`
porque el `<footer>` ya da contexto. Reservar `<nav>` evita saturar el mapa de landmarks.

## Estructura interna idiomática

Un menú es conceptualmente una **lista de destinos**, así que la estructura idiomática es una
`<ul>` de enlaces. Esto da a los lectores de pantalla el número de elementos ("lista de 3 elementos")
y permite estilarlo con CSS de forma predecible:

```html
<nav aria-label="Principal">
  <ul>
    <li><a href="/" aria-current="page">Inicio</a></li>
    <li><a href="/blog">Blog</a></li>
  </ul>
</nav>
```

`aria-current="page"` marca el enlace de la página actual, útil para estilarlo y para la asistencia.

## Varios nav en una página

Es válido tener varios `<nav>` (principal, de pie, breadcrumb). Cuando hay más de uno, **hay que
distinguirlos** con un nombre accesible para que el usuario sepa cuál es cuál al listarlos:

```html
<nav aria-label="Principal">…</nav>
<nav aria-label="Migas de pan">…</nav>
<nav aria-label="Pie">…</nav>
```

## Accesibilidad

> [!info] Landmark navigation
> `<nav>` genera un landmark "navigation". Los usuarios de lector de pantalla pueden listar todas las
> navegaciones de la página y saltar a la que quieran. El `aria-label` (o `aria-labelledby`) es lo
> que distingue una de otra en esa lista; sin él, varias "navigation" sin nombre son indistinguibles.

## Buenas prácticas

> [!tip] Recomendaciones
> - Reserva `<nav>` para la navegación primaria; no envuelvas cada grupo de enlaces.
> - Estructura el interior con `<ul>`/`<li>` aunque luego CSS los muestre en horizontal.
> - Nombra cada `<nav>` con `aria-label` si hay más de uno.
> - Marca el destino actual con `aria-current="page"`.

## Errores comunes

> [!warning] Trampas
> - **Abusar de `<nav>`**: envolver footers, listas de tags y cualquier conjunto de enlaces satura
>   los landmarks y resta utilidad a la navegación asistida.
> - **`<nav>` sin estructura de lista**: enlaces sueltos separados por `|` o espacios pierden la
>   semántica de lista y son peores para la asistencia.
> - **No es para "barra lateral"**: una barra lateral con contenido tangencial es
>   [[07 Contenido Complementario (aside) | `<aside>`]], no `<nav>` (la posición es cosa de CSS).

## Notas relacionadas

- [[02 Listas No Ordenadas (ul)]] — la estructura interna típica de un menú.
- [[01 Enlaces (a)]] — los enlaces que contiene un `<nav>`.
- [[01 Roles de Landmark]] — `<nav>` dentro del sistema de landmarks de accesibilidad.
