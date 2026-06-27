---
title: "remove"
aliases:
  - elemento.remove
  - eliminar nodo DOM
  - remove DOM
tags:
  - javascript
  - api/metodo
  - dom
draft: false
order: 1
---

# `remove`

> [!definicion]
> `elemento.remove()` desconecta el elemento del árbol DOM. Se invoca sobre el propio elemento — no requiere referencia al padre. Devuelve `undefined`. Si el elemento no está en el DOM (no tiene padre), la llamada se ignora silenciosamente sin lanzar error. El objeto JavaScript del elemento sigue existiendo en memoria mientras haya referencias activas.

```js
const aviso = document.querySelector(".aviso");
aviso.remove();
// El <div class="aviso"> ya no está en el DOM
// Pero la variable `aviso` sigue referenciando el objeto en memoria
console.log(aviso.isConnected); // false
```

## `isConnected` — verificar si el elemento está en el árbol

La propiedad booleana `isConnected` de cualquier nodo indica si está actualmente conectado al árbol del documento (o a un shadow DOM). Es el complemento natural de `remove()`.

```js
const el = document.querySelector(".panel");
console.log(el.isConnected); // true

el.remove();
console.log(el.isConnected); // false

document.body.appendChild(el); // reinsertar
console.log(el.isConnected); // true
```

`isConnected` también es `false` para elementos creados con `createElement` que todavía no se han insertado.

## Recetas comunes

### Cerrar un toast o notificación al hacer clic en el botón de cerrar

```js
document.querySelectorAll(".toast").forEach(toast => {
  toast.querySelector(".toast__cerrar").addEventListener("click", () => {
    toast.remove();
  });
});

// Con delegación de eventos (más eficiente para muchos toasts)
document.querySelector(".toast-contenedor").addEventListener("click", e => {
  const boton = e.target.closest(".toast__cerrar");
  if (boton) boton.closest(".toast").remove();
});
```

### Eliminar con animación antes de quitar del DOM

```js
function eliminarConAnimacion(el, duracion = 300) {
  el.style.transition = `opacity ${duracion}ms, transform ${duracion}ms`;
  el.style.opacity = "0";
  el.style.transform = "scale(0.9)";
  setTimeout(() => el.remove(), duracion);
}

eliminarConAnimacion(document.querySelector(".tarjeta"));
```

### Auto-eliminar un mensaje después de N segundos

```js
function mostrarMensajeTemporal(texto, duracion = 3000) {
  const el = document.createElement("div");
  el.className = "mensaje-temporal";
  el.textContent = texto;
  document.body.appendChild(el);
  setTimeout(() => el.remove(), duracion);
}
```

### Eliminar todos los elementos de un tipo

```js
// Eliminar todos los modales abiertos
document.querySelectorAll(".modal.activo").forEach(modal => modal.remove());

// Eliminar todos los errores de validación del formulario
document.querySelectorAll(".error-msg").forEach(el => el.remove());
```

## Reinserción después de `remove`

El nodo eliminado puede reinsertarse en el DOM — no se destruye:

```js
const sidebar = document.querySelector(".sidebar");
sidebar.remove();

// Más tarde, reinsertar
document.querySelector(".layout").appendChild(sidebar);
// El sidebar vuelve al DOM con todos sus hijos y atributos intactos
// (los event listeners también se conservan si el nodo no fue clonado)
```

## Cómo funciona por dentro

`remove` es un método de la interfaz `ChildNode` (disponible en `Element`, `CharacterData` y `DocumentType`). Internamente obtiene `this.parentNode` y, si no es `null`, llama al equivalente de `removeChild(this)` en el padre. El padre actualiza sus punteros internos (`firstChild`, `lastChild`, `childNodes`), y el nodo desconectado tiene su `parentNode` y `previousSibling`/`nextSibling` establecidos a `null`.

El objeto JavaScript del elemento no se destruye — el garbage collector solo lo recogerá cuando no haya ninguna referencia activa apuntando a él. Las referencias en variables, closures, colecciones y event listeners internos mantienen el objeto vivo.

> [!tip]
> Prefiere `remove()` sobre `removeChild()` en el 95% de los casos modernos. Es más conciso, no requiere acceder al padre, y no lanza errores si el elemento ya fue eliminado. Guarda `removeChild()` para cuando necesites explícitamente el valor de retorno (el nodo eliminado) para reutilizarlo.

> [!warning]
> Eliminar un elemento del DOM no elimina sus event listeners. Si el objeto permanece referenciado en memoria (por una variable, un closure o una estructura de datos), los listeners siguen activos y pueden causar memory leaks si el elemento se recrea repetidamente. Usa `AbortController` o llama a `removeEventListener` explícitamente antes de eliminar cuando sea relevante.

## Notas relacionadas

- [[02 removeChild]]
- [[index|Eliminar Elementos — Índice]]
- [[../06 Crear e Insertar Elementos/02 appendChild y append|appendChild y append]]
