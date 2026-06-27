---
title: Anclas internas (id) — Enlazar a secciones de la misma página
aliases:
  - fragment links
  - anclas
  - id anchor
tags:
  - html
  - api/concepto
  - navegacion
draft: false
order: 2
---

# Anclas Internas (id)

> [!definicion]
> Un **ancla interna** es un enlace que salta a una sección concreta de la misma página (o de otra), usando un fragmento `#id`. El enlace `<a href="#destino">` apunta al elemento que tiene `id="destino"`. Es la base de los índices de contenido y los "volver arriba".

```html
<a href="#metodos">Ir a la sección Métodos</a>
…
<h2 id="metodos">Métodos</h2>
```

Al hacer clic, el navegador desplaza la página hasta el elemento con ese `id` y actualiza la URL a `…/pagina#metodos`.

## Cómo funciona el fragmento

La parte de la URL tras `#` se llama **fragmento** (fragment identifier). No se envía al servidor: el navegador la usa localmente para localizar el elemento con ese `id` y desplazarse hasta él. Por eso una URL con `#seccion` puede compartirse y abre la página directamente en esa sección.

| `href` | Salta a |
|--------|---------|
| `#seccion` | El `id="seccion"` de la página actual |
| `otra.html#seccion` | La sección `seccion` de otra página |
| `#top` o `#` | El inicio de la página |

## Índice de contenidos

El patrón clásico es un índice navegable al inicio de un documento largo:

```html
<nav aria-label="Índice">
  <ul>
    <li><a href="#intro">Introducción</a></li>
    <li><a href="#uso">Uso</a></li>
    <li><a href="#api">Referencia de API</a></li>
  </ul>
</nav>
…
<section id="intro"><h2>Introducción</h2>…</section>
<section id="uso"><h2>Uso</h2>…</section>
<section id="api"><h2>Referencia de API</h2>…</section>
```

## Desplazamiento suave y compensación

Por defecto el salto es instantáneo. Se suaviza con CSS, y `scroll-margin-top` evita que un encabezado cabecera fija (sticky) tape el destino:

```css
html { scroll-behavior: smooth; }
:target { scroll-margin-top: 5rem; }   /* deja hueco para un header fijo */
```

El pseudo-selector `:target` permite, además, **resaltar** la sección a la que se acaba de saltar:

```css
:target { background: #2e2a3d; }
```

## Reglas del id

| Regla | Detalle |
|-------|---------|
| Único en la página | No puede repetirse un mismo `id` |
| Sin espacios | Usar guiones: `mi-seccion`, no `mi seccion` |
| Sensible a mayúsculas | `#Top` y `#top` son distintos |

Más sobre el atributo como identificador global en [[01 Identificación (id, class) | id y class]].

## Accesibilidad

> [!info] Foco y skip links
> Las anclas internas son la base de los [[03 Skip Links | skip links]] ("saltar al contenido"). Al saltar a un `id`, conviene que el destino pueda **recibir el foco** del teclado para que la navegación continúe desde ahí; un encabezado o un `<main>` con `tabindex="-1"` permite enfocarlo programáticamente tras el salto.

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon `id` descriptivos y estables en los encabezados de sección (sirven de URL compartible).
> - Añade `scroll-margin-top` si tienes una cabecera fija, para que no tape el destino.
> - Usa `scroll-behavior: smooth` con mesura (algunos usuarios prefieren saltos instantáneos; respeta `prefers-reduced-motion`).

## Errores comunes

> [!warning] Trampas
> - **`id` duplicados**: el navegador salta solo al primero; el resto queda inalcanzable.
> - **`id` con espacios o caracteres conflictivos**: rompen el enlace.
> - **Encabezado tapado** por un header fijo sin `scroll-margin-top`.

## Notas relacionadas

- [[01 Enlaces (a)]] — el elemento que usa el fragmento `#id`.
- [[01 Identificación (id, class)]] — el atributo `id` como identificador global.
- [[03 Skip Links]] — el caso de accesibilidad basado en anclas.
