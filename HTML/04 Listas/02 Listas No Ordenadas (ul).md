---
title: <ul> — Lista no ordenada
aliases:
  - ul
  - unordered list
tags:
  - html
  - api/elemento
  - semantica
elemento: ul
categoria: flujo
rol_implicito: list
vacio: false
draft: false
order: 2
---

# Listas No Ordenadas (ul)

> [!definicion]
> `<ul>` crea una lista **no ordenada**: un conjunto de elementos relacionados donde el orden **no** aporta significado (una lista de la compra, características de un producto, opciones de un menú). El navegador muestra una viñeta ante cada [[03 Elementos de Lista (li) | `<li>`]].

```html
<ul>
  <li>Manzanas</li>
  <li>Peras</li>
  <li>Plátanos</li>
</ul>
```

## ul vs. ol

| | `<ul>` | `<ol>` |
|--|--------|--------|
| Orden significativo | No | Sí |
| Marcador | Viñeta (•) | Número/letra |
| Reordenar cambia el sentido | No | Sí |

La viñeta es **decorativa**; cambiarla o quitarla con CSS no altera el significado, a diferencia de la numeración de [[01 Listas Ordenadas (ol) | `<ol>`]].

## Solo li como hijo directo

> [!warning] Nada de texto suelto en ul
> El único hijo directo válido de `<ul>` es `<li>` (y, técnicamente, elementos de script/template). Texto, `<p>` o `<div>` **directamente** dentro del `<ul>` —fuera de un `<li>`— es marcado inválido:
> ```html
> <!-- ❌ texto suelto en ul -->
> <ul>
>   Fruta:
>   <li>Manzana</li>
> </ul>
>
> <!-- ✅ todo dentro de li -->
> <ul>
>   <li>Manzana</li>
> </ul>
> ```
> Todo el contenido va **dentro** de un `<li>`, que sí admite cualquier contenido de flujo.

## Quitar las viñetas (menús)

El uso más frecuente de `<ul>` fuera de listas visibles es estructurar **menús de navegación**, donde se suprimen las viñetas con CSS pero se conserva la semántica de lista:

```html
<nav aria-label="Principal">
  <ul>
    <li><a href="/">Inicio</a></li>
    <li><a href="/blog">Blog</a></li>
  </ul>
</nav>
```

```css
nav ul { list-style: none; padding: 0; display: flex; gap: 1rem; }
```

> [!info] role="list" y list-style: none
> En algunos navegadores (Safari con VoiceOver), aplicar `list-style: none` puede hacer que la lista **deje de anunciarse como lista**. Si la semántica de lista importa para la accesibilidad y has quitado las viñetas, conviene reforzarla con `role="list"` en el `<ul>`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<ul>` para conjuntos sin orden y para la estructura interna de menús.
> - Todo el contenido dentro de `<li>`, nunca suelto en el `<ul>`.
> - Al quitar viñetas en menús, valora `role="list"` para preservar la semántica en todos los lectores.

## Errores comunes

> [!warning] Trampas
> - **Texto o bloques sueltos** como hijos directos del `<ul>`.
> - **Imitar una lista con `<p>` y `•`**: pierde la semántica de lista que la asistencia aprovecha.
> - **Usar `<ul>` cuando el orden importa**: eso es `<ol>`.

## Notas relacionadas

- [[01 Listas Ordenadas (ol)]] — cuando el orden sí importa.
- [[03 Elementos de Lista (li)]] — el ítem que contiene todo el contenido.
- [[03 Navegación (nav)]] — los menús se construyen con `<ul>`.
