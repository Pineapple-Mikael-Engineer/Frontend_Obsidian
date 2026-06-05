---
title: <li> — Elemento de lista
aliases:
  - li
  - list item
tags:
  - html
  - api/elemento
  - semantica
elemento: li
categoria: flujo
rol_implicito: listitem
vacio: false
draft: false
---

# Elementos de Lista (li)

> [!definicion]
> `<li>` representa un **ítem** dentro de una lista [[02 Listas No Ordenadas (ul) | `<ul>`]] u [[01 Listas Ordenadas (ol) | `<ol>`]]. Es el contenedor de cada elemento, y a diferencia de su lista padre, admite **cualquier contenido de flujo**: texto, párrafos, imágenes, enlaces e incluso otras listas.

```html
<ul>
  <li>Un ítem simple</li>
  <li>
    <strong>Un ítem complejo</strong>
    <p>con varios párrafos y</p>
    <ul>
      <li>una sublista</li>
    </ul>
  </li>
</ul>
```

## Contenido permitido

Mientras que el `<ul>`/`<ol>` solo acepta `<li>` como hijo, el `<li>` acepta de todo: es donde vive el contenido real. Esto incluye [[05 Listas Anidadas | listas anidadas]], que van **dentro** del `<li>`, no sueltas entre ítems.

## El atributo value (solo en ol)

Dentro de una lista ordenada, un `<li>` puede fijar su número con `value`; los siguientes continúan desde ahí:

```html
<ol>
  <li>Primero</li>          <!-- 1 -->
  <li value="5">Quinto</li> <!-- 5 -->
  <li>Sexto</li>            <!-- 6 -->
</ol>
```

En una `<ul>`, `value` no tiene efecto (no hay numeración).

## Estilo del marcador

El marcador (viñeta o número) se controla con CSS desde el `<li>` o la lista:

```css
li { margin-block: 0.25rem; }
li::marker { color: #cba6f7; }   /* color de la viñeta/número */
ul { list-style-type: square; }   /* tipo de viñeta */
```

El pseudo-elemento `::marker` permite estilar específicamente la viñeta o el número sin afectar al contenido del ítem.

## Buenas prácticas

> [!tip] Recomendaciones
> - Mete **todo** el contenido del ítem dentro del `<li>`, incluidas las sublistas.
> - Usa `value` en `<ol>` cuando necesites controlar un número concreto.
> - Para personalizar viñetas/números, usa `::marker` o `list-style`.

## Errores comunes

> [!warning] Trampas
> - **Sublista fuera del `<li>`**: una `<ul>` anidada va dentro del `<li>` padre, no entre dos `<li>`.
> - **Olvidar el `<li>`** y poner contenido directo en el `<ul>`: inválido.
> - **Numerar a mano** dentro del `<li>` en vez de dejar que `<ol>` lo haga.

## Notas relacionadas

- [[01 Listas Ordenadas (ol)]] — el contenedor ordenado y el atributo `value`.
- [[02 Listas No Ordenadas (ul)]] — el contenedor no ordenado.
- [[05 Listas Anidadas]] — sublistas dentro del `<li>`.
