---
title: Roles de Estructura — La organización del contenido
aliases:
  - document structure roles
tags:
  - html
  - api/concepto
  - a11y
draft: false
order: 2
---

# Roles de Estructura

> [!definicion]
> Los **roles de estructura** (document structure roles) describen la organización del **contenido**: que algo es una lista, un encabezado, un artículo, una tabla. Como los de landmark, los elementos HTML nativos ya los aportan; rara vez se escriben a mano.

```html
<!-- Roles de estructura implícitos -->
<ul>…</ul>        <!-- list -->
<li>…</li>        <!-- listitem -->
<h2>…</h2>        <!-- heading (nivel 2) -->
<article>…</article>  <!-- article -->
<table>…</table>  <!-- table -->
```

## Elemento ↔ rol de estructura

| Elemento | Rol implícito |
|----------|---------------|
| `<ul>` / `<ol>` | `list` |
| `<li>` | `listitem` |
| `<h1>`–`<h6>` | `heading` (con su `aria-level`) |
| `<article>` | `article` |
| `<table>` | `table` |
| `<th>` | `columnheader` / `rowheader` |
| `<figure>` | `figure` |

## Cuándo se necesita el rol explícito

> [!info] El caso de list-style: none
> Hay un caso real donde a veces hay que reponer el rol: al quitar las viñetas de una lista con `list-style: none`, **algunos navegadores** (Safari/VoiceOver) dejan de anunciarla como lista. Reponer `role="list"` lo restaura:
> ```html
> <ul role="list" style="list-style: none">…</ul>
> ```
> Es la excepción que confirma la regla: normalmente el elemento basta, pero ciertas combinaciones de CSS pueden "quitar" la semántica.

## heading y aria-level

Un encabezado nativo lleva su nivel implícito (`<h2>` → nivel 2). Si por alguna razón se construye un encabezado con otro elemento, se indica el nivel con `aria-level`:

```html
<div role="heading" aria-level="2">Título</div>
```

Pero, de nuevo, `<h2>` es siempre preferible: trae el nivel y el comportamiento sin esfuerzo.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa los elementos nativos; aportan el rol de estructura solos.
> - Repón `role="list"` solo si quitas las viñetas y necesitas garantizar el anuncio.
> - Prefiere `<h1>`–`<h6>` reales a `role="heading"` + `aria-level`.
> - No añadas roles redundantes (`<ul role="list">` solo cuando el CSS lo exija).

## Errores comunes

> [!warning] Trampas
> - **Roles redundantes** que repiten lo que el elemento ya dice.
> - **Olvidar `role="list"`** tras `list-style: none` en navegadores que lo requieren.
> - **Recrear encabezados** con `<div role="heading">` en vez de `<h2>`.

## Notas relacionadas

- [[01 Roles de Landmark]] — los roles de las regiones de página.
- [[02 Listas No Ordenadas (ul)]] — el caso de `list-style: none` y `role="list"`.
- [[01 Encabezados Jerárquicos (h1-h6)]] — los encabezados nativos.
