---
title: parentNode y parentElement — Subir al nodo padre
aliases:
  - parentNode
  - parentElement
tags:
  - javascript
  - api/metodo
  - dom
objeto: Node
tipo: metodo
retorna: Node | Element | null
muta: false
asincrono: false
draft: false
order: 1
---

# `parentNode` y `parentElement`

> [!definicion]
> `nodo.parentNode` devuelve el nodo padre directo del nodo en el árbol DOM; puede ser un `Element`, el `Document` o un `DocumentFragment`. `nodo.parentElement` devuelve el padre solo si es un `Element`; en cualquier otro caso devuelve `null`. Para la gran mayoría de elementos del DOM el resultado es idéntico, porque su padre suele ser otro elemento.

```js
const input = document.querySelector('input[name="email"]');

input.parentNode;    // → el HTMLElement padre (p. ej. <div class="campo">)
input.parentElement; // → el mismo HTMLElement padre

// La diferencia aflora en el nodo raíz
document.documentElement.parentNode;    // → document  (Document)
document.documentElement.parentElement; // → null       (Document no es Element)
```

## Firma

```text
Node.parentNode: Node | null
Node.parentElement: Element | null
```

Ambas son propiedades de solo lectura; no aceptan argumentos.

## Subir varios niveles

El encadenamiento permite ascender múltiples peldaños de una vez:

```js
const input = document.querySelector('.formulario input');

const abuelo = input.parentElement.parentElement;
// equivale a dos saltos hacia arriba en el árbol
```

Sin comprobaciones intermedias esto lanza `TypeError` si algún nivel es `null`. Para ascensos robustos con un selector arbitrario usar [[04 closest]] en su lugar.

## Receta: obtener el formulario que contiene un input

```html
<form id="login">
  <fieldset>
    <input type="email" id="correo" />
  </fieldset>
</form>
```

```js
const input = document.getElementById('correo');

// Idiomático y robusto — sube hasta encontrar el <form> más cercano
const form = input.closest('form');
form.id; // → "login"

// Equivalente con parentElement solo si la jerarquía es fija y conocida
const fieldset = input.parentElement;          // <fieldset>
const formDirecto = fieldset.parentElement;    // <form>
```

`closest('form')` es preferible cuando la profundidad puede variar entre revisiones de markup.

## Cómo funciona por dentro

> [!info]
> Cada nodo del árbol DOM mantiene internamente tres punteros: al padre, al primer hijo y al hermano siguiente. `parentNode` consulta directamente el puntero al padre sin recorrido alguno; la operación es O(1). `parentElement` añade una única comprobación de tipo: `padre.nodeType === Node.ELEMENT_NODE` (valor `1`). Si el padre es un `Document` (`nodeType === 9`) o un `DocumentFragment` (`nodeType === 11`), `parentElement` devuelve `null`.

La distinción es relevante solo en dos situaciones:

1. El nodo es `document.documentElement` (el `<html>`): su padre es el `Document`, no un `Element`.
2. El nodo pertenece a un `DocumentFragment` que aún no está insertado en el documento: su padre es el fragmento, no un `Element`.

## Diferencia resumida

| Propiedad | Tipo devuelto | Caso que difiere |
|---|---|---|
| `parentNode` | `Node` (Element, Document, DocumentFragment) | Hijo directo de `document` o `DocumentFragment` |
| `parentElement` | `Element` o `null` | Igual que antes, pero devuelve `null` si el padre no es Element |

> [!tip] Buenas prácticas
> - Usar `parentElement` en vez de `parentNode` en código de producto: el tipo de retorno más estrecho (`Element | null`) evita operar accidentalmente sobre un `Document` o `DocumentFragment`.
> - Comprobar que el resultado no es `null` antes de encadenar: `el.parentElement?.classList.add(...)`.
> - Para ascensos con criterio de selector usar `closest()`; para ascensos de profundidad fija y conocida, `parentElement` encadenado es suficiente.

> [!warning] Errores comunes
> - Asumir que `parentNode` y `parentElement` son siempre iguales e intercambiarlos sin pensar: la diferencia es silenciosa para el 99 % de los nodos pero produce `null` inesperado cuando el nodo es hijo directo de `document`.
> - Encadenar `parentElement.parentElement...` sin comprobar `null`: si el markup cambia y un nivel intermedio desaparece, la cadena lanza `TypeError: Cannot read properties of null`.
> - Llamar estas propiedades sobre `null`: si `querySelector` no encontró el nodo, el acceso explota. Siempre verificar la referencia inicial.

## Notas relacionadas

- [[index | Recorrer el DOM]]
- [[04 closest]]
- [[01 Selección de Elementos/index | Selección de Elementos]]
