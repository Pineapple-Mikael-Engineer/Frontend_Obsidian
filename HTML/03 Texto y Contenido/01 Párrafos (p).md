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
> `<p>` representa un **párrafo**: la unidad básica de texto en prosa. El navegador le añade un
> margen vertical por defecto que lo separa del párrafo siguiente.

```html
<p>El primer párrafo introduce la idea.</p>
<p>El segundo la desarrolla.</p>
```

## Reglas de anidamiento

| Permitido dentro de `<p>` | Prohibido dentro de `<p>` |
|---------------------------|---------------------------|
| Texto y elementos en línea (`<strong>`, `<a>`, `<code>`) | Otros `<p>` |
| `<br>`, `<span>`, `<img>` | Elementos de bloque (`<div>`, `<ul>`, `<h2>`, `<table>`) |

`<p>` solo admite **contenido fraseo** (en línea). Poner un `<div>` o una lista dentro cierra el
párrafo automáticamente y produce un DOM distinto del escrito.

> [!warning] El cierre automático sorprende
> El navegador cierra un `<p>` abierto en cuanto encuentra un elemento de bloque. Escribir
> `<p><ul>…</ul></p>` produce el párrafo vacío, la lista fuera, y un `</p>` huérfano ignorado. La
> estructura final no coincide con la intención.

> [!tip] No usar p para espaciar
> Un `<p></p>` vacío para "dar espacio" es mala práctica: el espaciado es trabajo de CSS (`margin`).
> `<p>` es para párrafos reales de contenido.

## Notas relacionadas

- [[02 Saltos de Línea (br)]] — salto dentro de un párrafo, no entre párrafos.
- [[index]] — el resto de elementos de texto que pueden ir dentro.
