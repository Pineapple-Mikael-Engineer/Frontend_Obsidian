---
title: insertAdjacentHTML — Insertar HTML en posición relativa sin destruir el contenido
aliases:
  - insertAdjacentHTML
  - Element.insertAdjacentHTML
tags:
  - javascript
  - api/metodo
  - dom
objeto: Element
tipo: metodo
retorna: undefined
muta: true
asincrono: false
draft: false
---

# insertAdjacentHTML

> [!definicion]
> `elemento.insertAdjacentHTML(posicion, html)` parsea el string `html` y lo inserta en la posición indicada relativa al elemento, **sin destruir el contenido existente** ni sus event listeners. Es preferible a `innerHTML +=` para insertar nodos sin reemplazar el árbol completo.

```js
const lista = document.querySelector("ul");

lista.insertAdjacentHTML("beforeend", "<li>Nuevo ítem</li>");
// Añade el <li> al final de <ul>, sin tocar los <li> existentes

lista.insertAdjacentHTML("afterbegin", "<li>Primer ítem</li>");
// Inserta antes del primer <li>
```

## Las 4 posiciones

```
<!-- beforebegin -->
<ul>
  <!-- afterbegin -->
  <li>Existente</li>
  <!-- beforeend -->
</ul>
<!-- afterend -->
```

| Posición | Dónde inserta |
|----------|---------------|
| `"beforebegin"` | Antes del elemento — como hermano anterior |
| `"afterbegin"` | Dentro del elemento, antes del primer hijo |
| `"beforeend"` | Dentro del elemento, después del último hijo |
| `"afterend"` | Después del elemento — como hermano siguiente |

`"beforebegin"` y `"afterend"` solo funcionan si el elemento tiene un nodo padre — lanzarán error en elementos desconectados del DOM.

## Ventaja sobre innerHTML +=

```js
const ul = document.querySelector("ul");

// MAL — innerHTML += destruye todos los <li> y sus listeners en cada vuelta
items.forEach(item => {
  ul.innerHTML += `<li data-id="${item.id}">${item.nombre}</li>`;
});

// BIEN — insertAdjacentHTML inserta sin afectar los nodos existentes
items.forEach(item => {
  ul.insertAdjacentHTML("beforeend", `<li data-id="${item.id}">${item.nombre}</li>`);
});
```

## Riesgo XSS

`insertAdjacentHTML` tiene el mismo riesgo que [[02 innerHTML (XSS)]]: si el string proviene de datos del usuario sin sanitizar, puede inyectar HTML malicioso.

```js
// PELIGROSO
ul.insertAdjacentHTML("beforeend", `<li>${inputDelUsuario}</li>`);

// SEGURO — usar insertAdjacentText para texto plano
ul.insertAdjacentText("beforeend", inputDelUsuario);
// El texto se escapa automáticamente — equivalente a textContent
```

## Métodos hermanos

| Método | Qué inserta | Parsea HTML |
|--------|-------------|-------------|
| `insertAdjacentHTML(pos, string)` | HTML parseado | Sí |
| `insertAdjacentText(pos, string)` | Texto escapado | No — seguro ante XSS |
| `insertAdjacentElement(pos, nodo)` | Nodo DOM existente | — |

`insertAdjacentElement` mueve un nodo DOM ya creado, sin parsear nada. Es equivalente a `before()` / `after()` / `prepend()` / `append()`, pero con la misma API de posición que `insertAdjacentHTML`.

## Recetas comunes

```js
// Añadir item al final de una lista
ul.insertAdjacentHTML("beforeend", `<li>${nombre}</li>`);

// Insertar un banner antes de una sección
seccion.insertAdjacentHTML("beforebegin", '<div class="banner">Aviso</div>');

// Añadir un label después de un input
input.insertAdjacentHTML("afterend", `<span class="error">${mensaje}</span>`);

// Insertar un spinner dentro de un botón, antes del texto
btn.insertAdjacentHTML("afterbegin", '<span class="spinner"></span>');
```

## Cómo funciona por dentro

El motor usa fragment parsing con el elemento de destino como contexto, igual que `innerHTML`. El parser crea un `DocumentFragment` y luego lo inserta en la posición indicada mediante operaciones de árbol. Los `<script>` insertados con `insertAdjacentHTML` **sí se ejecutan** en algunos navegadores (a diferencia de `innerHTML`), aunque el comportamiento varía; en Firefox históricamente los ejecutaba.

> [!warning]
> A diferencia de `innerHTML`, algunos navegadores ejecutan `<script>` insertados con `insertAdjacentHTML`. Para contenido potencialmente hostil, siempre sanitizar con DOMPurify antes de usar este método.

## Notas relacionadas

- [[02 innerHTML (XSS)]]
- [[01 textContent]]
- [[06 Crear e Insertar Elementos/index | Crear e Insertar Elementos]]
- [[03 Modificar Contenido/index | Modificar Contenido]]
