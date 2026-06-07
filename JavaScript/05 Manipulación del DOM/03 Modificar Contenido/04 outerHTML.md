---
title: outerHTML — Leer y reemplazar el elemento completo como string HTML
aliases:
  - outerHTML
  - Element.outerHTML
tags:
  - javascript
  - api/metodo
  - dom
objeto: Element
tipo: metodo
retorna: string
muta: true
asincrono: false
draft: false
---

# outerHTML

> [!definicion]
> `elemento.outerHTML` serializa el elemento **completo** — incluyendo su tag de apertura, atributos, contenido y tag de cierre — como string HTML. Al escribir, reemplaza el elemento en el DOM por el HTML indicado, eliminando el nodo original del árbol. Cualquier referencia JS al elemento original queda huérfana.

```js
const btn = document.querySelector("button");

// Leer — incluye el propio elemento
console.log(btn.outerHTML);
// → '<button class="cta" disabled>Enviar</button>'

// Comparar con innerHTML — solo el contenido interno
console.log(btn.innerHTML);
// → 'Enviar'
```

## Escritura: reemplazar un elemento

Al asignar a `outerHTML`, el motor parsea el string como HTML en el contexto del nodo padre y reemplaza el elemento en el árbol. El nodo original se elimina del DOM.

```js
const btn = document.querySelector("button");
btn.outerHTML = '<a href="/enviar" class="cta">Enviar</a>';

// btn sigue siendo una referencia al <button> eliminado
// btn ya NO está en el DOM
console.log(btn.isConnected); // → false

// Para obtener el nuevo elemento, es necesario buscarlo de nuevo
const enlace = document.querySelector("a.cta");
```

> [!warning]
> `outerHTML` no devuelve el nuevo nodo creado — devuelve `undefined`. Si se necesita la referencia al elemento resultante, se debe obtener con `querySelector` u otro método de selección después del reemplazo.

## Trampa: referencia huérfana

```js
const el = document.querySelector("#item");
el.outerHTML = '<div id="item" class="nuevo">Actualizado</div>';

// el apunta al nodo eliminado — no al nuevo <div>
el.classList.add("activo"); // No lanza error, pero no tiene efecto visible
// El elemento real en el DOM no se modifica
```

## Receta: reemplazar un elemento por otro tipo

`outerHTML` es la forma más directa de cambiar el tipo de elemento (p. ej., de `<button>` a `<a>`, de `<div>` a `<section>`) sin manipular el árbol manualmente:

```js
function convertirAEnlace(boton, href) {
  const clases = boton.className;
  const texto = boton.textContent;
  boton.outerHTML = `<a href="${href}" class="${clases}">${texto}</a>`;
  // Retornar la nueva referencia si se necesita
  return document.querySelector(`a[href="${href}"]`);
}
```

## Cómo funciona por dentro

Al leer, `outerHTML` equivale a serializar el nodo con el algoritmo HTML5 de serialización, que incluye el nodo raíz (a diferencia de `innerHTML`, que solo serializa los descendientes). Al escribir, el motor encuentra el nodo padre, parsea el string en ese contexto (fragment parsing con el padre como contexto), y reemplaza el nodo en la lista de hijos del padre. Si el elemento no tiene padre (es el nodo raíz o está desconectado del DOM), la asignación lanza `DOMException: "This element has no parent node"`.

> [!info]
> Intentar asignar a `document.documentElement.outerHTML` lanza una excepción en la mayoría de navegadores. La propiedad es de solo lectura para el elemento raíz del documento.

## Notas relacionadas

- [[02 innerHTML (XSS)]]
- [[05 insertAdjacentHTML]]
- [[06 Crear e Insertar Elementos/index | Crear e Insertar Elementos]]
- [[03 Modificar Contenido/index | Modificar Contenido]]
