---
title: dataset — Acceso a atributos data-* como propiedades camelCase
aliases:
  - dataset
  - DOMStringMap
  - HTMLElement.dataset
  - data attributes
tags:
  - javascript
  - api/metodo
  - dom
objeto: HTMLElement
tipo: metodo
retorna: DOMStringMap
muta: true
asincrono: false
draft: false
order: 5
---

# dataset

> [!definicion]
> `elemento.dataset` devuelve un `DOMStringMap` live que expone los atributos `data-*` del elemento como propiedades camelCase. La conversión es automática: `data-user-id` → `dataset.userId`. Los valores siempre son strings — la lectura y escritura operan sobre los atributos `data-*` del elemento.

```js
// HTML: <button data-action="eliminar" data-user-id="42" data-is-admin="true">
const btn = document.querySelector("button");

// Leer
console.log(btn.dataset.action);   // → "eliminar"
console.log(btn.dataset.userId);   // → "42" (string, no número)
console.log(btn.dataset.isAdmin);  // → "true" (string, no boolean)

// Escribir — crea o actualiza el atributo data-*
btn.dataset.userId = 99;           // crea/actualiza data-user-id="99"
btn.dataset.nuevoValor = "test";   // crea data-nuevo-valor="test"
```

## Conversión de nombres: kebab-case ↔ camelCase

La conversión es bidireccional y automática:

| Atributo HTML | Propiedad dataset |
|---------------|-------------------|
| `data-id` | `dataset.id` |
| `data-user-id` | `dataset.userId` |
| `data-is-admin` | `dataset.isAdmin` |
| `data-fecha-nacimiento` | `dataset.fechaNacimiento` |
| `data-url-base` | `dataset.urlBase` |

La regla: cada guion seguido de letra minúscula se convierte en esa letra en mayúscula (camelCase). En dirección inversa, cada mayúscula se convierte en `-letra-minúscula`.

## CRUD completo sobre data-*

```js
// Leer
const id = el.dataset.productId;          // → string o undefined

// Escribir / crear
el.dataset.productId = "123";             // → crea data-product-id="123"

// Eliminar
delete el.dataset.productId;             // → elimina el atributo data-product-id

// Comprobar existencia
if ("productId" in el.dataset) {
  // el atributo data-product-id existe
}

// Comprobar con undefined
if (el.dataset.productId !== undefined) { ... }
```

## Receta: delegación de eventos con data-*

El patrón más común — marcar elementos con datos para handlers sin closures individuales:

```html
<ul id="lista">
  <li data-action="editar" data-id="1">Item 1 <button>Editar</button></li>
  <li data-action="eliminar" data-id="2">Item 2 <button>Eliminar</button></li>
</ul>
```

```js
document.querySelector("#lista").addEventListener("click", (e) => {
  const li = e.target.closest("li");
  if (!li) return;

  const { action, id } = li.dataset; // destructuring del DOMStringMap
  // action → "editar" o "eliminar"
  // id → "1" o "2" (strings — convertir si se necesita número)

  if (action === "eliminar") eliminar(Number(id));
  if (action === "editar")   editarElemento(Number(id));
});
```

## Iteración

```js
// for...in — itera las claves camelCase
for (const clave in el.dataset) {
  console.log(clave, el.dataset[clave]);
}

// Object.entries — convierte a array de pares [clave, valor]
Object.entries(el.dataset).forEach(([clave, valor]) => {
  console.log(`data-${clave}: ${valor}`);
});
```

## dataset vs getAttribute para data-*

```js
// Equivalentes al leer:
el.dataset.userId;              // → "42"
el.getAttribute("data-user-id"); // → "42"

// dataset es más ergonómico para lectura/escritura frecuente
// getAttribute es útil cuando el nombre del atributo es dinámico:
const atrib = `data-${nombreVariable}`;
el.getAttribute(atrib);
```

## Cómo funciona por dentro

`DOMStringMap` es un objeto proxy live sobre los atributos `data-*` del elemento. Internamente delega en `getAttribute`/`setAttribute`/`removeAttribute` aplicando la conversión camelCase ↔ kebab-case. Es una vista en tiempo real — cambios en el atributo HTML se reflejan inmediatamente en `dataset` y viceversa.

> [!warning]
> Los valores de `dataset` son siempre strings. Al leer `el.dataset.cantidad` se obtiene `"5"`, no `5`. Convertir explícitamente cuando se necesiten tipos numéricos o booleanos: `Number(el.dataset.cantidad)` o `el.dataset.activo === "true"`.

> [!tip]
> `dataset` no existe en `SVGElement` ni en nodos genéricos — es exclusivo de `HTMLElement`. Para SVG, usar `getAttribute`/`setAttribute` directamente con el nombre `data-*` completo.

## Notas relacionadas

- [[01 getAttribute y setAttribute]]
- [[04 classList]]
- [[06 Manejo de Eventos/index | Delegación de Eventos]]
- [[04 Modificar Atributos/index | Modificar Atributos]]
