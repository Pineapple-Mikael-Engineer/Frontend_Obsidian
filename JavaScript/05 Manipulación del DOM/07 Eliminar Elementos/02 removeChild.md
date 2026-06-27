---
title: "removeChild"
aliases:
  - padre.removeChild
  - eliminar hijo DOM
  - removeChild DOM
tags:
  - javascript
  - api/metodo
  - dom
draft: false
order: 2
---

# `removeChild`

> [!definicion]
> `padre.removeChild(hijo)` elimina `hijo` del árbol DOM, desconectándolo de `padre`. Devuelve el nodo eliminado. Lanza `NotFoundError` si `hijo` no es un hijo directo de `padre`. La API clásica equivalente de `remove()` — más verbosa pero útil cuando se quiere conservar la referencia al nodo eliminado para reutilizarlo.

```js
const lista = document.querySelector("ul");
const primerItem = lista.firstElementChild;

const nodoEliminado = lista.removeChild(primerItem);
// primerItem ya no está en el DOM
// nodoEliminado === primerItem — la referencia al nodo sigue siendo válida
console.log(nodoEliminado.textContent); // sigue accesible
```

## Firma completa

```js
const nodoDesconectado = padre.removeChild(hijo);
// Devuelve el nodo eliminado — esto es lo que lo distingue de remove()
```

## Cuándo usar `removeChild` en lugar de `remove`

`removeChild` tiene una sola ventaja sobre `remove()`: devuelve el nodo eliminado. Esto es útil para el patrón de **mover un nodo de un contenedor a otro** sin perder la referencia.

```js
// Con removeChild — una referencia limpia al nodo movido
const nodo = contenedor1.removeChild(elemento);
contenedor2.appendChild(nodo);

// Con remove — necesitas la referencia de antemano
const nodo = elemento; // guardar antes de remove
elemento.remove();
contenedor2.appendChild(nodo);
// Ambos funcionan; removeChild es más explícito sobre la intención
```

## Recetas comunes

### Mover un elemento de un contenedor a otro

```js
const papelera = document.querySelector(".papelera");
const activos = document.querySelector(".activos");

function moverAPapelera(elemento) {
  const nodo = activos.removeChild(elemento);
  papelera.appendChild(nodo);
}

document.querySelectorAll(".item").forEach(item => {
  item.querySelector(".btn-eliminar").addEventListener("click", () => {
    moverAPapelera(item);
  });
});
```

### Vaciar un contenedor eficientemente con `removeChild`

```js
// Método clásico — elimina un hijo por iteración
const contenedor = document.querySelector(".lista");
while (contenedor.firstChild) {
  contenedor.removeChild(contenedor.firstChild);
}

// Alternativas modernas equivalentes
contenedor.innerHTML = "";          // simple pero reparsea
contenedor.replaceChildren();       // la forma más limpia y moderna
contenedor.textContent = "";        // válida, pero elimina solo text nodes de forma directa
```

### Eliminar el último hijo repetidamente (pila visual)

```js
const pila = document.querySelector(".pila");
const btnDeshacer = document.querySelector(".btn-deshacer");

const historial = [];

btnDeshacer.addEventListener("click", () => {
  if (pila.lastElementChild) {
    const nodo = pila.removeChild(pila.lastElementChild);
    historial.push(nodo); // guardar para posible reinserción
  }
});
```

### Reinsertar el nodo eliminado (deshacer)

```js
const nodo = lista.removeChild(lista.lastElementChild);
// ... el usuario hace clic en "Deshacer" ...
lista.appendChild(nodo); // reinsertar al final
// El nodo conserva todos sus atributos, hijos y listeners
```

## Comparativa `remove` vs `removeChild`

| Aspecto | `elemento.remove()` | `padre.removeChild(hijo)` |
|---|---|---|
| Se invoca sobre | El propio elemento | El padre |
| Requiere ref. al padre | No | Sí |
| Devuelve | `undefined` | El nodo eliminado |
| Error si no tiene padre | No (silencioso) | Sí (`NotFoundError`) si `hijo` no es hijo de `padre` |
| API | Moderna | Clásica |
| Caso de uso principal | Eliminar y olvidar | Eliminar para reutilizar/mover |

## Cómo funciona por dentro

`removeChild` es un método de la interfaz `Node`. Antes de eliminar, valida que el nodo pasado sea un hijo directo del nodo sobre el que se invoca — si no lo es, lanza `DOMException` con tipo `NotFoundError`. Una vez validado, el motor actualiza la lista doblemente enlazada de nodos hijos: ajusta `previousSibling` y `nextSibling` de los vecinos del nodo eliminado, actualiza `firstChild` y `lastChild` del padre si corresponde, y establece `parentNode` del nodo eliminado a `null`. El nodo queda en memoria como un árbol desconectado.

> [!tip]
> El patrón `while (el.firstChild) el.removeChild(el.firstChild)` para vaciar un contenedor es el más eficiente en términos de memoria en entornos donde importa, porque no reasigna `innerHTML` ni dispara el parser HTML. Sin embargo, para la mayoría de las aplicaciones `replaceChildren()` es la opción moderna equivalente y más legible.

> [!warning]
> `removeChild` lanza `NotFoundError` si el nodo que intentas eliminar no es hijo directo del padre que usas. El error más común: usar `removeChild` sobre un ancestro lejano en lugar del padre inmediato. Usa `hijo.parentNode.removeChild(hijo)` si no tienes la referencia al padre, o simplemente `hijo.remove()`.

## Notas relacionadas

- [[01 remove]]
- [[index|Eliminar Elementos — Índice]]
- [[../06 Crear e Insertar Elementos/07 replaceChild y replaceWith|replaceChild y replaceWith]]
