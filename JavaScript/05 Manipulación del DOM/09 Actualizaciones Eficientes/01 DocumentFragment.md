---
title: DocumentFragment
aliases:
  - DocumentFragment
  - document.createDocumentFragment
  - fragment
tags:
  - javascript
  - api/metodo
  - dom
draft: false
---

# `DocumentFragment`

> [!definicion]
> Un `DocumentFragment` es un nodo del DOM que vive fuera del árbol del documento. Los elementos que se le añaden tampoco forman parte del DOM activo. Al insertar el fragment en el DOM, todos sus hijos se mueven al árbol en **una sola operación** — un único reflow en lugar de uno por cada hijo insertado individualmente.

```js
const frag = document.createDocumentFragment();

datos.forEach(d => {
  const li = document.createElement('li');
  li.textContent = d.nombre;
  frag.appendChild(li);          // fuera del DOM — sin reflow
});

lista.appendChild(frag);         // un solo reflow al insertar todo
```

## Por qué reduce los reflows

Cada modificación al DOM activo puede forzar al motor a recalcular el layout. Al construir los nodos dentro de un `DocumentFragment`, el motor no tiene que actualizar el árbol de renderizado hasta que el fragment se inserta. El coste de N inserciones individuales se reduce a 1.

```js
// MAL — N reflows (uno por insertAdjacentElement)
datos.forEach(d => {
  const li = document.createElement('li');
  li.textContent = d.nombre;
  lista.appendChild(li);  // reflow en cada iteración
});

// BIEN — 1 reflow con DocumentFragment
const frag = document.createDocumentFragment();
datos.forEach(d => {
  const li = document.createElement('li');
  li.textContent = d.nombre;
  frag.appendChild(li);
});
lista.appendChild(frag);
```

## El fragment queda vacío tras la inserción

Cuando el `DocumentFragment` se inserta en el DOM, sus hijos se **mueven** (no se copian). El fragment queda vacío y puede reutilizarse. Si se necesitan los nodos también en otro lugar, clonarlos antes con `cloneNode(true)`.

```js
const frag = document.createDocumentFragment();
const li = document.createElement('li');
frag.appendChild(li);

lista.appendChild(frag);
console.log(frag.childNodes.length); // 0 — li se movió al DOM
```

## Receta: lista de 100 items

```js
async function renderizarLista(datos) {
  const frag = document.createDocumentFragment();

  datos.forEach(({ id, nombre, descripcion }) => {
    const li = document.createElement('li');
    li.dataset.id = id;

    const titulo = document.createElement('strong');
    titulo.textContent = nombre;

    const desc = document.createElement('span');
    desc.textContent = descripcion;

    li.append(titulo, ' — ', desc);
    frag.appendChild(li);
  });

  lista.replaceChildren(frag); // reemplaza todo el contenido en una operación
}
```

## Alternativa moderna: `el.append(...nodos)`

`Element.append()` acepta múltiples nodos y strings en una sola llamada, lo que también reduce los reflows respecto a insertar uno a uno. Para listas pequeñas o medianas, esta forma es más concisa:

```js
const items = datos.map(d => {
  const li = document.createElement('li');
  li.textContent = d.nombre;
  return li;
});

lista.append(...items); // múltiples nodos, una sola operación
```

Para volúmenes muy grandes (cientos o miles de nodos), `DocumentFragment` sigue siendo preferible por mayor compatibilidad y control.

## Cómo funciona por dentro

Un `DocumentFragment` es un tipo especial de nodo (`Node.DOCUMENT_FRAGMENT_NODE`) que no tiene un `parentNode` real. Sus hijos no forman parte del árbol de renderizado, por lo que el motor no calcula su layout ni los pinta. Solo cuando el fragment se inserta en el árbol real el motor procesa el subtree completo de una vez.

> [!tip]
> `document.createDocumentFragment()` no acepta HTML como string. Para parsear HTML directamente a nodos, usar `el.innerHTML` en un elemento temporal o `DOMParser`. El fragment es para nodos ya construidos programáticamente.

> [!warning]
> `DocumentFragment` no hereda de `Element` sino de `Node`. No tiene atributos, clases ni estilos. Si se necesita un contenedor real para aplicar estilos, usar un `<div>` o `<span>` en su lugar.

## Notas relacionadas

- [[03 Reflows y Repaints]] — por qué importa reducir el número de reflows
- [[02 requestAnimationFrame]] — sincronizar actualizaciones con el ciclo de renderizado
- [[09 Actualizaciones Eficientes/index|Actualizaciones Eficientes]]
