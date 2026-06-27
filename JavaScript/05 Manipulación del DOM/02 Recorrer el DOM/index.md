---
title: Recorrer el DOM — Navegación por el árbol
aliases:
  - recorrer el DOM
  - navegar el árbol DOM
  - tree traversal DOM
tags:
  - javascript
  - api/concepto
  - dom
objeto: Node
tipo: concepto
muta: false
asincrono: false
draft: false
order: 2
---

# Recorrer el DOM

> [!definicion]
> Recorrer el DOM es navegar por el árbol de nodos usando las propiedades de parentesco que expone la interfaz `Node` y `Element`: relaciones padre–hijo–hermano. A diferencia de `querySelector`, que busca por selector CSS desde la raíz, la navegación por relaciones sigue los punteros internos del árbol y es O(1) por salto.

```js
const lista = document.querySelector('ul');

// Subir al padre
const contenedor = lista.parentElement;

// Hijos solo Elements
for (const item of lista.children) {
  console.log(item.textContent);
}

// Hermano siguiente Element
const siguiente = lista.nextElementSibling;

// Ancestro más cercano con clase
const seccion = lista.closest('.seccion');
```

## La distinción clave: `*Node` vs `*Element`

Las propiedades del DOM se dividen en dos familias según qué tipos de nodos devuelven:

- Las propiedades `*Node` (`parentNode`, `childNodes`, `nextSibling`, `previousSibling`) operan sobre cualquier nodo del árbol: **Elements**, **nodos de texto** (incluyendo espacios en blanco e indentación), y **comentarios** HTML.
- Las propiedades `*Element` (`parentElement`, `children`, `nextElementSibling`, `previousElementSibling`) filtran y devuelven **solo nodos de tipo Element** (`nodeType === 1`).

En HTML formateado con indentación, entre dos etiquetas adyacentes siempre hay un nodo de texto con `"\n"`. Por eso `elemento.nextSibling` suele devolver ese nodo de texto invisible, mientras que `elemento.nextElementSibling` salta directamente al siguiente elemento.

## Tabla de propiedades

| Propiedad | Disponible en | Devuelve | Incluye nodos de texto |
|---|---|---|---|
| `parentNode` | `Node` | `Node` (Element, Document, DocumentFragment) | — (es padre, no hijos) |
| `parentElement` | `Node` | `Element` o `null` | — |
| `childNodes` | `Node` | `NodeList` live | sí |
| `children` | `Element` | `HTMLCollection` live | no |
| `firstChild` | `Node` | `Node` o `null` | sí |
| `lastChild` | `Node` | `Node` o `null` | sí |
| `firstElementChild` | `Element` | `Element` o `null` | no |
| `lastElementChild` | `Element` | `Element` o `null` | no |
| `childElementCount` | `Element` | `number` | no |
| `nextSibling` | `Node` | `Node` o `null` | sí |
| `previousSibling` | `Node` | `Node` o `null` | sí |
| `nextElementSibling` | `Element` | `Element` o `null` | no |
| `previousElementSibling` | `Element` | `Element` o `null` | no |
| `closest(selector)` | `Element` | `Element` o `null` | no |
| `contains(nodo)` | `Node` | `boolean` | — |

## Cuándo usar cada familia

Usar la familia `*Element` en casi todos los casos de código de producto: el HTML siempre tiene nodos de texto entre etiquetas y operar con `*Node` requiere comprobar `nodeType` en cada paso. La familia `*Node` es útil cuando se trabaja deliberadamente con nodos de texto o comentarios (editores de texto enriquecido, parsers, clonación de nodos).

## Notas relacionadas

- [[01 parentNode y parentElement]]
- [[02 childNodes y children]]
- [[03 Hermanos (nextSibling, previousSibling)]]
- [[04 closest]]
- [[05 contains]]
- [[01 Selección de Elementos/index | Selección de Elementos]]
