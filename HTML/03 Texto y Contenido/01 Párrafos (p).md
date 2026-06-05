---
title: <p> — Párrafo de texto
aliases:
  - paragraph
tags:
  - html
  - api/elemento
  - semantica
elemento: p
categoria: flujo
rol_implicito: paragraph
vacio: false
draft: false
---

# Párrafos (p)

> [!definicion]
> `<p>` representa un **párrafo**: la unidad básica de texto en prosa. El navegador le añade un margen
> vertical por defecto que lo separa del párrafo siguiente, y solo admite **contenido en línea**
> (texto y elementos de fraseo), nunca otros bloques.

```html
<p>El primer párrafo introduce la idea.</p>
<p>El segundo la desarrolla, con <strong>una palabra importante</strong>
   y un <a href="/ref">enlace</a>.</p>
```

## Qué puede contener (y qué no)

`<p>` solo acepta **contenido de fraseo** (en línea). Esta restricción es la fuente de muchos bugs:

| Permitido dentro de `<p>` | Prohibido dentro de `<p>` |
|---------------------------|---------------------------|
| Texto | Otro `<p>` |
| `<strong>`, `<em>`, `<a>`, `<code>`, `<span>` | `<div>`, `<section>` |
| `<br>`, `<img>`, `<time>`, `<abbr>` | `<ul>`, `<ol>`, `<table>` |
| `<mark>`, `<q>`, `<sub>`, `<sup>` | `<h1>`–`<h6>`, `<blockquote>`, `<figure>` |

## El cierre automático

> [!warning] El parser cierra el p ante un bloque
> El navegador **cierra automáticamente** un `<p>` abierto en cuanto encuentra un elemento de bloque.
> Esto produce un DOM distinto del escrito:
> ```html
> <!-- Lo que escribes -->
> <p>Texto<ul><li>item</li></ul>más texto</p>
>
> <!-- Lo que el navegador construye -->
> <p>Texto</p>
> <ul><li>item</li></ul>
> más texto
> <p></p>   <!-- p vacío del </p> huérfano -->
> ```
> El `<p>` se cierra antes de la lista, la lista queda fuera, "más texto" queda suelto y el `</p>`
> final crea un párrafo vacío. Por eso una lista o una cita **nunca** van dentro de un párrafo.

## Espaciado: margen, no párrafos vacíos

El navegador aplica `margin-block` por defecto a `<p>`. Para controlar la separación se usa CSS, no
párrafos o `<br>` vacíos:

```css
p { margin-block: 0 1em; }   /* sin margen arriba, 1em abajo */
```

Un `<p></p>` vacío "para dar espacio" es semánticamente incorrecto y los lectores de pantalla pueden
anunciarlo de forma confusa.

## Buenas prácticas

> [!tip] Recomendaciones
> - Un `<p>` por idea o párrafo real de prosa.
> - El espaciado entre párrafos se controla con `margin` en CSS, no con `<br>` ni párrafos vacíos.
> - Para una lista de elementos, sal del párrafo y usa [[02 Listas No Ordenadas (ul) | `<ul>`/`<ol>`]].

## Errores comunes

> [!warning] Trampas
> - **Anidar bloques** (listas, tablas, divs) dentro de `<p>`: el parser los expulsa.
> - **`<p>` vacíos para maquetar**: usa márgenes CSS.
> - **Olvidar el cierre** en HTML manual: aunque `</p>` es opcional en algunos casos, cerrarlo evita
>   sorpresas con el cierre automático.

## Notas relacionadas

- [[02 Saltos de Línea (br)]] — salto dentro de un párrafo, no entre párrafos.
- [[03 Línea Horizontal (hr)]] — separación temática entre bloques de párrafos.
- [[index]] — los elementos en línea que sí pueden ir dentro de un `<p>`.
