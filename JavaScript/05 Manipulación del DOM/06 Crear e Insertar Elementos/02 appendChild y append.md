---
title: "appendChild y append"
aliases:
  - appendChild
  - append DOM
  - insertar último hijo
tags:
  - javascript
  - api/metodo
  - dom
draft: false
order: 2
---

# `appendChild` y `append`

> [!definicion]
> `padre.appendChild(nodo)` inserta un nodo como **último hijo** del padre y devuelve el nodo insertado. `padre.append(...nodosOStrings)` es la versión moderna: acepta múltiples argumentos (nodos o strings) y los inserta todos al final, pero devuelve `undefined`. Ambos **mueven** el nodo si ya está en el DOM — no lo copian.

```js
const lista = document.querySelector("ul");
const li = document.createElement("li");
li.textContent = "Último item";

// Clásico
const nodoInsertado = lista.appendChild(li); // devuelve li

// Moderno — múltiples argumentos, acepta strings
lista.append("Texto directo", document.createElement("li"), otrNodo);
```

## Comparativa detallada

| Característica | `appendChild(nodo)` | `append(...items)` |
|---|---|---|
| Argumentos | Un solo nodo | Múltiples nodos y/o strings |
| Devuelve | El nodo insertado | `undefined` |
| Acepta strings | No (lanza `TypeError`) | Sí (se convierten a nodos de texto) |
| Posición de inserción | Último hijo | Último hijo |
| API | Clásica (IE9+) | Moderna (ES2015+) |

`append` con un string es equivalente a crear un `TextNode` e insertarlo — el string se escapa automáticamente.

## Comportamiento de "mover"

Si el nodo que se pasa ya existe en el árbol del documento, ambos métodos lo **mueven** a la nueva posición. El nodo se elimina de su ubicación actual y se reinserta como último hijo del nuevo padre. No se crea ninguna copia.

```js
const a = document.querySelector(".origen .item");
const destino = document.querySelector(".destino");

destino.appendChild(a);
// .item ya no está en .origen — fue movido a .destino
```

Este comportamiento es útil para reordenar elementos sin necesidad de `removeChild` explícito, pero puede ser sorprendente si no se espera.

## Recetas comunes

### Añadir múltiples elementos al final

```js
const fragmento = ["Manzana", "Banana", "Cereza"].map(nombre => {
  const li = document.createElement("li");
  li.textContent = nombre;
  return li;
});

lista.append(...fragmento);
// O con un DocumentFragment para una sola operación de reflow
```

### Mover un elemento al final de su contenedor

```js
const lista = document.querySelector(".lista");
const primero = lista.firstElementChild;

// Mover el primer elemento al final
lista.appendChild(primero); // se desconecta del inicio y se reconecta al final
```

### Usar el valor de retorno de `appendChild`

```js
// appendChild devuelve el nodo, útil para encadenar configuraciones
const span = contenedor.appendChild(document.createElement("span"));
span.textContent = "texto añadido";
span.classList.add("etiqueta");
// El elemento ya está en el DOM y tiene las propiedades aplicadas
```

### Añadir strings y nodos mezclados

```js
const p = document.querySelector("p");
const strong = document.createElement("strong");
strong.textContent = "importante";

p.append("El concepto ", strong, " es clave.");
// Resultado: <p>El concepto <strong>importante</strong> es clave.</p>
```

## Cómo funciona por dentro

Cuando se llama a `appendChild` o `append`, el motor del DOM:

1. Si el nodo tiene un padre actual, lo desconecta llamando internamente al equivalente de `removeChild` — el padre anterior actualiza su lista de hijos.
2. Conecta el nodo como último hijo del nuevo padre, actualizando los punteros `parentNode`, `previousSibling` del nodo y `lastChild` del padre.
3. Si el nodo es un `DocumentFragment`, todos sus hijos se mueven al padre — el fragmento queda vacío. Los fragmentos se usan precisamente para hacer una única operación de inserción con múltiples nodos.
4. El motor marca el subárbol como modificado y programa un repintado para el próximo frame.

> [!tip]
> Para insertar muchos elementos de una vez (por ejemplo, rellenar una lista con datos de una API), crea un `DocumentFragment`, añade todos los elementos al fragmento, y luego inserta el fragmento en el DOM con un solo `appendChild`. Esto reduce a uno el número de reflows, en lugar de uno por elemento.

> [!warning]
> `append` con un string inserta texto plano — no HTML. `lista.append("<li>texto</li>")` insertará el string literal como texto visible, no un elemento `<li>`. Si necesitas insertar HTML directamente, usa `insertAdjacentHTML` (con datos de confianza) o crea los elementos con `createElement`.

## Notas relacionadas

- [[01 createElement y createTextNode]]
- [[03 prepend]]
- [[04 insertBefore]]
- [[index|Crear e Insertar Elementos — Índice]]
