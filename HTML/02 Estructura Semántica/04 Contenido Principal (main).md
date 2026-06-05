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
> sitio. Excluye todo lo que se repite en cada página (cabecera, navegación global, pie). Debe haber
> **un solo `<main>` visible** por documento.

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

## Qué entra y qué no

La prueba es: *¿esto cambia de una página a otra del sitio?* Si se repite igual en todas, no es
contenido principal.

| Dentro de `<main>` | Fuera de `<main>` |
|--------------------|-------------------|
| El artículo, el formulario, el listado | El logo y el menú global (`<header>`, `<nav>`) |
| El `<h1>` de la página | El pie con enlaces legales (`<footer>`) |
| Contenido único de esta vista | Banners de cookies, navegación repetida |

## Reglas

| Regla | Razón |
|-------|-------|
| Uno solo visible por página | El "contenido principal" es único por definición |
| No anidarlo en `article`, `aside`, `header`, `footer`, `nav` | `<main>` es de nivel superior |
| Puede haber varios si todos menos uno tienen `hidden` | Útil en SPAs que precargan vistas ocultas |

## El destino del "saltar al contenido"

`<main>` es el ancla de accesibilidad más útil de la página. Dos mecanismos lo aprovechan:

1. **Skip link** — un enlace al inicio del `<body>` que salta la navegación repetida:
   ```html
   <a href="#contenido" class="skip-link">Saltar al contenido</a>
   …
   <main id="contenido">…</main>
   ```
2. **Tecla rápida del lector de pantalla** para ir directo al landmark "main".

Ambos evitan que un usuario de teclado o lector tenga que recorrer el menú entero en cada página.

## Accesibilidad

> [!info] Landmark main
> `<main>` expone el landmark "main", del que solo debe haber uno. Es donde apuntan los
> [[03 Skip Links | skip links]] y la navegación por regiones. Reproducir esto con
> `<div role="main">` es posible pero innecesario y más frágil: el elemento nativo ya trae el rol.

## Buenas prácticas

> [!tip] Recomendaciones
> - Coloca `<main>` como hijo directo del `<body>`, hermano de `<header>`/`<footer>`.
> - Dale un `id` (`id="contenido"`) para que el skip link pueda apuntarlo.
> - Pon el `<h1>` de la página dentro del `<main>`.

## Errores comunes

> [!warning] Trampas
> - **Varios `<main>` visibles**: invalida el documento y confunde a la asistencia.
> - **Usar `<main>` como contenedor de maquetación**: no se elige por conveniencia de CSS; para
>   agrupar por estilo se usa un `<div>`. Su valor es el significado "aquí empieza lo importante".
> - **Meter la navegación repetida dentro**: el menú global va fuera, en `<header>`/`<nav>`.

## Notas relacionadas

- [[04 Cuerpo (body)]] — `<main>` vive directamente bajo el `<body>`.
- [[03 Skip Links]] — el enlace de salto que apunta al `<main>`.
- [[06 Artículos (article)]] — el contenido autónomo que suele ir dentro.
