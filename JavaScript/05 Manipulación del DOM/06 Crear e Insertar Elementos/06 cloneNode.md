---
title: "cloneNode"
aliases:
  - nodo.cloneNode
  - clonar elemento DOM
  - deep clone DOM
tags:
  - javascript
  - api/metodo
  - dom
draft: false
order: 6
---

# `cloneNode`

> [!definicion]
> `nodo.cloneNode(deep = false)` crea y devuelve una copia del nodo. Con `deep = true`, la copia incluye todos los descendientes (hijos, nietos, etc.) y sus atributos. Con `deep = false` (o sin argumento), solo se copia el nodo raíz sin ningún hijo. El clon no tiene padre — hay que insertarlo en el DOM para que sea visible.

```js
const tarjeta = document.querySelector(".tarjeta");

// Clon superficial — solo el elemento raíz, sin hijos
const clonVacio = tarjeta.cloneNode(false);
// <div class="tarjeta"></div>  — sin contenido interior

// Clon profundo — elemento + todos sus descendientes
const clonCompleto = tarjeta.cloneNode(true);
// Copia exacta del árbol de la tarjeta

// El clon no está en el DOM hasta que se inserta
document.querySelector(".grid").appendChild(clonCompleto);
```

## Qué se clona y qué no

| Aspecto | ¿Se clona? | Notas |
|---|---|---|
| El nodo raíz | Siempre | Tipo de elemento, tag |
| Atributos del raíz | Sí | `id`, `class`, `data-*`, `style`, `src`, etc. |
| Hijos y descendientes | Solo con `deep = true` | Texto, elementos anidados, etc. |
| Atributos de los hijos | Solo con `deep = true` | Cuando los hijos se clonan, sus atributos van incluidos |
| Event listeners (`addEventListener`) | No | Los listeners no se copian bajo ninguna circunstancia |
| Propiedades JS del objeto | No | Solo atributos del DOM, no propiedades arbitrarias |
| Estado de formulario | Parcialmente | `value` de `<input>` no, pero el atributo `value` del HTML sí |

## El problema de los `id` duplicados

`cloneNode` copia el atributo `id`. Si el clon se inserta en el mismo documento, habrá dos elementos con el mismo `id`, lo que es inválido en HTML y rompe `getElementById`, `querySelector("#id")`, los `<label for="">`, los anchors, y la accesibilidad.

```js
const clon = original.cloneNode(true);

// Siempre limpiar o actualizar el id antes de insertar
clon.id = "tarjeta-" + nuevoId;

// Para IDs en elementos internos, también hay que actualizarlos
clon.querySelectorAll("[id]").forEach(el => {
  el.id = el.id + "-clon";
});

grid.appendChild(clon);
```

## Alternativa moderna: `<template>` + `cloneNode`

El elemento `<template>` está diseñado exactamente para este caso de uso: definir HTML reutilizable que no se renderiza hasta que se clona e inserta.

```html
<template id="tmpl-tarjeta">
  <article class="tarjeta">
    <h2 class="tarjeta__titulo"></h2>
    <p class="tarjeta__desc"></p>
  </article>
</template>
```

```js
const template = document.getElementById("tmpl-tarjeta");

function crearTarjeta({ id, titulo, descripcion }) {
  const clon = template.content.cloneNode(true); // deep = true siempre aquí
  // template.content es un DocumentFragment
  const article = clon.querySelector("article");
  article.id = "tarjeta-" + id;
  clon.querySelector(".tarjeta__titulo").textContent = titulo;
  clon.querySelector(".tarjeta__desc").textContent = descripcion;
  return clon; // DocumentFragment listo para insertar
}

grid.appendChild(crearTarjeta({ id: 1, titulo: "Producto A", descripcion: "..." }));
```

## Recetas comunes

### Duplicar una tarjeta de producto en un grid

```js
function duplicarTarjeta(tarjetaOriginal, nuevoId, nuevoTitulo) {
  const clon = tarjetaOriginal.cloneNode(true);
  clon.id = "tarjeta-" + nuevoId;
  clon.querySelector(".tarjeta__titulo").textContent = nuevoTitulo;
  tarjetaOriginal.parentNode.appendChild(clon);
  return clon;
}
```

### Clonar una fila de tabla para una nueva entrada

```js
const filaPlantilla = document.querySelector("tr.plantilla");
const nuevaFila = filaPlantilla.cloneNode(true);
nuevaFila.classList.remove("plantilla");
nuevaFila.removeAttribute("hidden");
nuevaFila.querySelector(".celda-nombre").textContent = "Nuevo usuario";
tabla.querySelector("tbody").appendChild(nuevaFila);
```

## Cómo funciona por dentro

`cloneNode` es un método de la interfaz `Node`. Para un clon superficial, el motor crea un nuevo objeto del mismo tipo, copia todos los atributos del nodo original y devuelve el nuevo nodo sin padre. Para un clon profundo, hace lo mismo recursivamente para cada descendiente, reconstruyendo el árbol completo en memoria.

Los event listeners se almacenan fuera del DOM en estructuras internas del motor del navegador — no forman parte del árbol de nodos ni de sus atributos, por eso no se clonan. Para clonar comportamiento junto con estructura, necesitas reaplicar los listeners manualmente o usar delegación de eventos en un ancestro común (un listener en el padre que captura eventos de todos los hijos mediante `event.target`).

> [!tip]
> Para plantillas repetibles, prefiere el elemento `<template>` sobre clonar nodos del DOM activo. El contenido de `<template>` no se renderiza ni ejecuta scripts/imágenes hasta que se clona — es más eficiente y semánticamente correcto que mantener un nodo oculto en el documento.

> [!warning]
> Los event listeners no se clonan. Si el original tiene listeners de clic, el clon no los tendrá. Usa delegación de eventos (un listener en el contenedor padre) para que todos los clones respondan a eventos automáticamente sin necesidad de reatacharlos individualmente.

## Notas relacionadas

- [[01 createElement y createTextNode]]
- [[02 appendChild y append]]
- [[07 replaceChild y replaceWith]]
- [[index|Crear e Insertar Elementos — Índice]]
