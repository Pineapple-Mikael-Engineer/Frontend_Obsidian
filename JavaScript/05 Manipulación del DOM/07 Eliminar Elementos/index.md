---
title: "Eliminar Elementos — Índice"
aliases:
  - Índice Eliminar Elementos
  - DOM eliminar JS
tags:
  - javascript
  - dom
  - index
draft: false
---

# Eliminar Elementos

Hay dos formas de eliminar un elemento del árbol DOM desde JavaScript. La diferencia principal es la ergonomía y si se necesita conservar el nodo eliminado.

| Método | Se invoca sobre | Devuelve | Requiere ref. al padre | API |
|---|---|---|---|---|
| `elemento.remove()` | El propio elemento | `undefined` | No | Moderna |
| `padre.removeChild(hijo)` | El padre | El nodo eliminado | Sí | Clásica |

En la práctica moderna, `remove()` cubre la gran mayoría de los casos por ser más directo. `removeChild()` sigue siendo útil cuando se quiere conservar y reutilizar el nodo eliminado — su valor de retorno es la referencia al nodo desconectado.

Después de eliminar un elemento del DOM, el objeto JavaScript correspondiente sigue existiendo en memoria mientras haya referencias activas a él. Puede reinsertarse con cualquier método de inserción. La propiedad `el.isConnected` indica si el elemento está actualmente en el árbol del documento.

## Notas relacionadas

- [[01 remove]]
- [[02 removeChild]]
