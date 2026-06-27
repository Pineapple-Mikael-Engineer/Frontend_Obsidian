---
title: Roles de Landmark — Regiones navegables de la página
aliases:
  - landmark roles
tags:
  - html
  - api/concepto
  - a11y
draft: false
order: 1
---

# Roles de Landmark

> [!definicion]
> Los **roles de landmark** identifican las grandes **regiones** de una página (navegación, contenido principal, pie), permitiendo a los usuarios de lector de pantalla **saltar directamente** a ellas. La buena noticia: los elementos semánticos de HTML5 ya generan estos landmarks **sin necesidad de ARIA**.

```html
<!-- Estos elementos generan landmarks automáticamente -->
<header>…</header>   <!-- banner -->
<nav>…</nav>         <!-- navigation -->
<main>…</main>       <!-- main -->
<aside>…</aside>     <!-- complementary -->
<footer>…</footer>   <!-- contentinfo -->
```

## Elemento HTML ↔ rol de landmark

| Elemento HTML | Rol de landmark |
|---------------|-----------------|
| `<header>` (hijo del body) | `banner` |
| `<nav>` | `navigation` |
| `<main>` | `main` |
| `<aside>` | `complementary` |
| `<footer>` (hijo del body) | `contentinfo` |
| `<section>` con nombre | `region` |
| `<form>` con nombre | `form` |

> [!info] Usa el elemento, no el role
> Como cada elemento semántico **ya genera** su landmark, casi nunca hace falta escribir `role="navigation"`: basta con `<nav>`. El rol ARIA explícito solo se usa cuando, por alguna razón heredada, no se puede usar el elemento nativo. La regla: `<main>` sí, `<div role="main">` solo como último recurso.

## Cómo los usa la asistencia

Los lectores de pantalla ofrecen un **menú de landmarks**: el usuario pulsa una tecla y obtiene la lista de regiones ("banner, navigation, main, contentinfo") para saltar a la que quiera. Esto convierte una página larga en navegable por zonas, sin recorrerla linealmente.

## Distinguir landmarks repetidos

Cuando hay **varios** del mismo tipo (dos `<nav>`, varios `<aside>`), hay que nombrarlos para diferenciarlos en la lista:

```html
<nav aria-label="Principal">…</nav>
<nav aria-label="Migas de pan">…</nav>
```

Sin `aria-label`, el usuario oye "navigation, navigation" sin saber cuál es cuál.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa los elementos semánticos (`<nav>`, `<main>`…), no los roles ARIA equivalentes.
> - Un solo `banner`, `main` y `contentinfo` por página.
> - Nombra con `aria-label` los landmarks repetidos.
> - Asegura que **todo** el contenido cae dentro de algún landmark (sin texto huérfano).

## Errores comunes

> [!warning] Trampas
> - **`role="navigation"` en un `<div>`** cuando `<nav>` haría lo mismo.
> - **Landmarks repetidos sin nombre**: indistinguibles.
> - **Varios `main`/`banner`**: solo debe haber uno de cada.

## Notas relacionadas

- [[02 Estructura Semántica/index]] — los elementos que generan estos landmarks.
- [[02 Roles de Estructura]] — los roles del contenido, nivel por debajo.
- [[03 Skip Links]] — saltar al landmark `main`.
