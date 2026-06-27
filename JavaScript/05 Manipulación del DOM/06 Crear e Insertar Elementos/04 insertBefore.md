---
title: "insertBefore"
aliases:
  - padre.insertBefore
  - insertar antes de nodo
  - insertBefore DOM
tags:
  - javascript
  - api/metodo
  - dom
draft: false
order: 4
---

# `insertBefore`

> [!definicion]
> `padre.insertBefore(nuevoNodo, nodoReferencia)` inserta `nuevoNodo` como hijo de `padre`, posicionándolo inmediatamente **antes** de `nodoReferencia`. Devuelve el nodo insertado. Si `nodoReferencia` es `null`, el nodo se inserta al final — comportamiento idéntico a `appendChild`. Lanza `NotFoundError` si `nodoReferencia` no es hijo directo de `padre`.

```js
const lista = document.querySelector("ul");
const nuevoItem = document.createElement("li");
nuevoItem.textContent = "Insertado en posición 2";

const referencia = lista.children[1]; // el segundo hijo actual
lista.insertBefore(nuevoItem, referencia);
// nuevoItem queda antes de referencia
```

## Firma completa

```js
const nodoInsertado = padre.insertBefore(nuevoNodo, nodoReferencia);
// nodoInsertado === nuevoNodo — devuelve el nodo insertado
```

## Casos especiales

```js
// Si nodoReferencia es null → equivale a appendChild
padre.insertBefore(nodo, null); // inserta al final

// Si nuevoNodo ya está en el DOM → se mueve
padre.insertBefore(nodoExistente, referencia);

// Error si nodoReferencia no es hijo de padre
otroElemento.insertBefore(nodo, hijoDeOtroPadre); // NotFoundError
```

## Cuándo usar `insertBefore` vs alternativas modernas

`insertBefore` es API clásica (disponible desde IE5). Hoy, `before()` y `after()` son más ergonómicas para la mayoría de los casos. Sin embargo, `insertBefore` sigue siendo útil cuando:

- Ya se tiene la referencia al padre y al nodo de referencia directamente (sin necesidad de navegar al padre desde el nodo de referencia).
- Se necesita el nodo de retorno (ninguno de los métodos modernos lo devuelve).
- Se trabaja con código que debe ser compatible con entornos que no tienen los métodos modernos del DOM.

## Recetas comunes

### Insertar un elemento en una posición específica por índice

```js
function insertarEnPosicion(padre, nodo, indice) {
  const referencia = padre.children[indice] ?? null;
  // null si indice >= padre.children.length → inserta al final
  return padre.insertBefore(nodo, referencia);
}

insertarEnPosicion(lista, nuevoLi, 0); // al inicio (equivale a prepend)
insertarEnPosicion(lista, nuevoLi, 2); // en posición 2
```

### Insertar un separador antes de cada elemento de grupo

```js
grupos.forEach(grupo => {
  const separador = document.createElement("hr");
  contenedor.insertBefore(separador, grupo);
});
```

### Insertar un nodo de advertencia antes de un campo con error

```js
function mostrarError(campo, mensaje) {
  const aviso = document.createElement("span");
  aviso.className = "error-msg";
  aviso.textContent = mensaje;
  campo.parentNode.insertBefore(aviso, campo);
}
```

## Cómo funciona por dentro

`insertBefore` es un método de la interfaz `Node`. Antes de insertar, el motor valida que `nodoReferencia` sea un hijo directo de `padre` — si no lo es, lanza una excepción `DOMException` de tipo `NotFoundError`. Si la validación pasa, el nodo es desconectado de su padre actual (si lo tiene) y reconectado al árbol en la posición indicada. El motor actualiza los punteros de la lista enlazada interna de nodos: `previousSibling`, `nextSibling`, `parentNode` del nodo insertado, y `firstChild`/`lastChild` del padre si corresponde.

> [!tip]
> El patrón `padre.insertBefore(nodo, padre.firstChild)` es el equivalente de `prepend` en API clásica. El patrón `padre.insertBefore(nodo, null)` es idéntico a `appendChild`. Estos dos hechos simplifican el modelo mental: `insertBefore` es el único método de inserción clásico que cubre todas las posiciones posibles dentro de un padre.

> [!warning]
> `insertBefore` requiere que tengas la referencia al **padre** — no al nodo de referencia. Si solo tienes el nodo de referencia, usa `nodoRef.before(nuevoNodo)` en su lugar, que es más conciso y no requiere acceder a `parentNode`.

## Notas relacionadas

- [[03 prepend]]
- [[05 before y after]]
- [[02 appendChild y append]]
- [[index|Crear e Insertar Elementos — Índice]]
