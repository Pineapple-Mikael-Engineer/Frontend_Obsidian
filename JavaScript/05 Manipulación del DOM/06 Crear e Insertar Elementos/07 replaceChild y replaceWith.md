---
title: "replaceChild y replaceWith"
aliases:
  - padre.replaceChild
  - nodo.replaceWith
  - replaceChildren
  - reemplazar elemento DOM
tags:
  - javascript
  - api/metodo
  - dom
draft: false
order: 7
---

# `replaceChild` y `replaceWith`

> [!definicion]
> `padre.replaceChild(nuevoNodo, nodoAReemplazar)` sustituye un hijo existente por un nodo nuevo. Es la API clásica: requiere la referencia al padre y devuelve el nodo reemplazado (eliminado). `nodo.replaceWith(...nodosOStrings)` es la versión moderna: se invoca sobre el nodo a reemplazar directamente, acepta múltiples argumentos y strings, y devuelve `undefined`. `padre.replaceChildren(...items)` reemplaza todos los hijos del padre de una vez.

```js
// API clásica
const viejo = document.querySelector(".viejo");
const nuevo = document.createElement("div");
nuevo.textContent = "Reemplazado";
const eliminado = viejo.parentNode.replaceChild(nuevo, viejo);
// eliminado === viejo — se puede reutilizar

// API moderna
const objetivo = document.querySelector(".objetivo");
const sustituto = document.createElement("section");
sustituto.textContent = "Versión nueva";
objetivo.replaceWith(sustituto);
// No devuelve nada útil
```

## Comparativa de los tres métodos

| Aspecto | `padre.replaceChild(nuevo, viejo)` | `nodo.replaceWith(...items)` | `padre.replaceChildren(...items)` |
|---|---|---|---|
| Se invoca sobre | El padre | El nodo a reemplazar | El padre |
| Requiere ref. al padre | Sí | No | Sí |
| Reemplaza | Un hijo específico | El propio nodo | Todos los hijos |
| Acepta strings | No | Sí | Sí |
| Devuelve | El nodo eliminado | `undefined` | `undefined` |
| Sin argumentos | N/A | Elimina el nodo | Vacía el contenedor |
| API | Clásica | Moderna | Moderna |

## `replaceChildren` — vaciar y rellenar

`replaceChildren` sin argumentos vacía el elemento de forma eficiente — es la alternativa moderna a `innerHTML = ""` o al bucle `while(el.firstChild) el.removeChild(el.firstChild)`.

```js
const lista = document.querySelector("ul");

// Vaciar completamente
lista.replaceChildren();
// Equivalente a: lista.innerHTML = ""  pero sin parsear HTML

// Reemplazar todo el contenido con nuevos elementos
const items = datos.map(d => {
  const li = document.createElement("li");
  li.textContent = d.nombre;
  return li;
});
lista.replaceChildren(...items);
// Más eficiente que vaciar + append en loop
```

## Recetas comunes

### Reemplazar un placeholder con contenido real

```js
const skeleton = document.querySelector(".skeleton");
const contenido = document.createElement("article");
contenido.classList.add("tarjeta");
contenido.innerHTML = `<h2>${datos.titulo}</h2><p>${datos.resumen}</p>`;
// datos.titulo y datos.resumen son de confianza del servidor
skeleton.replaceWith(contenido);
```

### Actualizar una lista completa con nuevos datos (sin borrar y reinsertar individualmente)

```js
async function actualizarLista(endpoint) {
  const datos = await fetch(endpoint).then(r => r.json());
  const lista = document.querySelector(".resultados");
  const nuevosItems = datos.map(item => {
    const li = document.createElement("li");
    li.dataset.id = item.id;
    li.textContent = item.nombre;
    return li;
  });
  lista.replaceChildren(...nuevosItems);
}
```

### Usar el retorno de `replaceChild` para mover el nodo a otro sitio

```js
const contenedor1 = document.querySelector(".contenedor-a");
const contenedor2 = document.querySelector(".contenedor-b");
const placeholder = document.createElement("div");
placeholder.className = "placeholder";

// Quitar el primer hijo de contenedor1 y poner un placeholder
const nodoMovido = contenedor1.replaceChild(placeholder, contenedor1.firstElementChild);
// nodoMovido es el hijo extraído

// Insertar el nodo movido en contenedor2
contenedor2.appendChild(nodoMovido);
```

### `replaceWith` con múltiples nodos (expansión de un elemento)

```js
// Reemplazar un <p> por varios párrafos
const parrafo = document.querySelector("p.expandir");
const nuevosParrafos = ["Primero.", "Segundo.", "Tercero."].map(t => {
  const p = document.createElement("p");
  p.textContent = t;
  return p;
});
parrafo.replaceWith(...nuevosParrafos);
```

## Cómo funciona por dentro

`replaceChild` es un método de `Node`. Internamente ejecuta un `insertBefore` del nuevo nodo en la posición del nodo a reemplazar, seguido de la desconexión del nodo reemplazado del árbol. El nodo eliminado queda en memoria si hay referencias JS a él — no se destruye automáticamente.

`replaceWith` es un método de `ChildNode`. Obtiene `this.parentNode`, determina la posición actual de `this` en la lista de hijos, inserta los nuevos nodos en esa posición, y luego elimina `this`. Si `this.parentNode` es `null`, la operación no hace nada.

`replaceChildren` desconecta todos los hijos actuales de una vez (actualizando `firstChild`, `lastChild`, `childNodes`) y luego inserta los nuevos. Al hacerlo en una sola operación, el motor puede optimizar el número de recálculos de layout.

> [!tip]
> Para actualizar listas de datos completas (resultados de búsqueda, feeds, tablas), `replaceChildren(...nuevosItems)` es la opción más eficiente y legible: vacía y rellena en una sola operación, sin manipular el DOM elemento por elemento.

> [!warning]
> `replaceChild` lanza `NotFoundError` si el nodo a reemplazar no es hijo directo del padre. Si solo tienes el nodo a reemplazar (no su padre), usa `nodo.replaceWith(nuevo)` para evitar el error y el acceso explícito a `parentNode`.

## Notas relacionadas

- [[02 appendChild y append]]
- [[06 cloneNode]]
- [[../07 Eliminar Elementos/01 remove|remove]]
- [[index|Crear e Insertar Elementos — Índice]]
