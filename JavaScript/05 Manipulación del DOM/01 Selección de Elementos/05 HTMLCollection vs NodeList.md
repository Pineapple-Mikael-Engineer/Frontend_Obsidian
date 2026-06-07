---
title: HTMLCollection vs NodeList — Colecciones del DOM
aliases:
  - HTMLCollection
  - NodeList
  - colecciones DOM
tags:
  - javascript
  - api/concepto
  - dom
objeto: document
tipo: concepto
muta: false
asincrono: false
draft: false
---

# `HTMLCollection` vs `NodeList`

> [!definicion]
> `HTMLCollection` y `NodeList` son los dos tipos de colecciones que retornan los métodos de selección del DOM. La diferencia clave es la naturaleza de la colección: `HTMLCollection` es siempre **live** (refleja cambios futuros del DOM); `NodeList` puede ser **live** (`childNodes`) o **estática** (`querySelectorAll`). Ambas son array-like pero no son arrays.

```js
// HTMLCollection — live
const hc  = document.getElementsByClassName('item');    // live
const hc2 = element.children;                          // live

// NodeList — estática
const nl  = document.querySelectorAll('.item');         // static

// NodeList — live (excepción)
const nl2 = element.childNodes;                        // live
```

## Tabla comparativa

| Característica | `HTMLCollection` | `NodeList` (static) | `NodeList` (live) |
|---|---|---|---|
| Devuelta por | `getElementsBy*`, `children` | `querySelectorAll` | `childNodes` |
| Live / static | live | static | live |
| Tipos de nodo | solo `Element` | cualquier `Node` | cualquier `Node` |
| Acceso por índice | si | si | si |
| Acceso por nombre/id | si | no | no |
| `forEach` nativo | no | si | si |
| `entries`, `keys`, `values` | no | si | si |
| Iterable con `for...of` | si (navegadores modernos) | si | si |

## Live vs Static: consecuencias prácticas

**Live** significa que la colección es una vista activa sobre el árbol DOM. El motor la actualiza automáticamente cuando se insertan o eliminan nodos que coinciden con el criterio original.

```js
const items = document.getElementsByClassName('item');   // live
console.log(items.length);   // → 3

const nuevo = document.createElement('div');
nuevo.className = 'item';
document.body.appendChild(nuevo);

console.log(items.length);   // → 4  (actualización automática)
```

**Static** significa que la colección es una instantánea. El DOM puede cambiar después de la llamada sin afectar a la `NodeList`:

```js
const items = document.querySelectorAll('.item');   // static
console.log(items.length);   // → 3

document.querySelector('.item').remove();
console.log(items.length);   // → 3  (no cambia)
```

## Tipos de nodo

`HTMLCollection` solo contiene nodos de tipo `Element` (etiquetas HTML). `NodeList` puede contener cualquier tipo de nodo: `Element`, `Text`, `Comment`, `ProcessingInstruction`, etc.

```js
// childNodes incluye nodos de texto (espacios en blanco) y comentarios
const nodos = document.body.childNodes;
for (const nodo of nodos) {
  console.log(nodo.nodeType, nodo.nodeName);
  // → 3 "#text"     (nodo de texto — espacios en blanco)
  // → 1 "DIV"       (Element)
  // → 8 "#comment"  (comentario HTML)
}

// children solo incluye Elements
const elementos = document.body.children;
for (const el of elementos) {
  console.log(el.tagName);   // solo etiquetas, nunca texto ni comentarios
}
```

## Conversión a Array

Ninguna de las dos es un `Array`. Para acceder a `map`, `filter`, `reduce`, `find`, etc.:

```js
const nl = document.querySelectorAll('p');
const hc = document.getElementsByTagName('p');

// Spread (requiere iterable — ambas lo son en navegadores modernos)
const arr1 = [...nl];
const arr2 = [...hc];

// Array.from (más explícito, también acepta map function)
const textos = Array.from(nl, el => el.textContent.trim());
```

## La trampa del bucle con colección live

Modificar el DOM mientras se itera una colección live con índice numérico altera la longitud en cada iteración, causando que se salten o repitan elementos:

```js
// BUG clásico: eliminar todos los elementos con clase "item"
const items = document.getElementsByClassName('item');

// INCORRECTO — se saltan elementos
for (let i = 0; i < items.length; i++) {
  items[i].remove();
  // Cuando se elimina items[0], items[1] pasa a ser items[0].
  // En la siguiente iteración i=1, se salta el nuevo items[0].
}

// CORRECTO — convertir a array antes de iterar
[...items].forEach(el => el.remove());

// CORRECTO — iterar de atrás hacia adelante
for (let i = items.length - 1; i >= 0; i--) {
  items[i].remove();
}
```

## Cómo funciona por dentro

`HTMLCollection` es un objeto que mantiene una referencia a los criterios de búsqueda originales (clase, tag) y a la raíz de búsqueda. Cada acceso a `length` o a un miembro por índice dispara una consulta sobre el árbol del DOM usando los índices internos del motor. No existe una lista materializada que haya que actualizar: la "actualización" es simplemente que la próxima consulta al árbol ya ve el estado nuevo.

`NodeList` estática (devuelta por `querySelectorAll`) es diferente: el motor materializa la lista de nodos coincidentes en el momento de la llamada, copiando las referencias en una estructura interna. No registra ningún observer sobre el árbol; las mutaciones posteriores son irrelevantes para esa estructura.

`NodeList` live (`childNodes`) funciona de manera similar a `HTMLCollection`: es una vista sobre el árbol que refleja el estado actual en cada acceso.

> [!tip] Buenas prácticas
> - Usar `querySelectorAll` cuando se va a iterar o manipular la colección de inmediato y no se necesita actualización automática.
> - Usar `getElementsBy*` cuando se necesita una vista que refleje cambios futuros (p. ej. para observar cuántos elementos activos hay en tiempo real).
> - Siempre convertir a array antes de iterar con operaciones que muten el DOM.
> - Preferir `element.children` sobre `element.childNodes` cuando solo se necesitan elementos (evita el ruido de nodos de texto).

> [!warning] Errores comunes
> - Llamar `.forEach()` sobre `HTMLCollection`: lanza `TypeError: hc.forEach is not a function`. Solo `NodeList` tiene `forEach` nativo.
> - Asumir que `childNodes` es estática: es live, igual que `HTMLCollection`.
> - Iterar con índice sobre `getElementsBy*` mientras se muta el DOM: se saltan o repiten elementos.
> - Usar `for...in` sobre colecciones DOM: itera también por propiedades enumerables heredadas del prototipo, no solo por los índices numéricos.

## Notas relacionadas

- [[index | Selección de Elementos]]
- [[02 getElementsByClassName y TagName]]
- [[04 querySelectorAll]]
- [[03 querySelector]]
