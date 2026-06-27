---
title: "prepend"
aliases:
  - padre.prepend
  - insertar primer hijo
  - prepend DOM
tags:
  - javascript
  - api/metodo
  - dom
draft: false
order: 3
---

# `prepend`

> [!definicion]
> `padre.prepend(...nodosOStrings)` inserta los nodos y strings proporcionados como los **primeros hijos** del elemento padre, antes de cualquier hijo existente. Acepta múltiples argumentos en una sola llamada. Devuelve `undefined`. Si alguno de los nodos ya está en el DOM, se mueve en lugar de copiarse.

```js
const lista = document.querySelector("ul");

// Insertar un nodo como primer hijo
const li = document.createElement("li");
li.textContent = "Primer elemento";
lista.prepend(li);

// Insertar múltiples elementos y strings de una vez
const encabezado = document.createElement("li");
encabezado.textContent = "Encabezado";
encabezado.classList.add("lista__encabezado");

lista.prepend(encabezado, "Texto introductorio");
// encabezado queda primero, luego el nodo de texto, luego los hijos previos
```

## Relación con `insertBefore`

`prepend` es equivalente a `insertBefore` apuntando al primer hijo, pero más conciso y capaz de recibir múltiples argumentos:

```js
// Equivalentes (para un solo nodo)
padre.prepend(nodo);
padre.insertBefore(nodo, padre.firstChild);

// prepend puede insertar varios de una vez — insertBefore solo uno
padre.prepend(nodo1, nodo2, "texto");
// No hay equivalente directo con insertBefore — requiere varias llamadas
```

## Orden de los argumentos múltiples

Cuando se pasan varios argumentos, se insertan en el mismo orden en que aparecen como argumentos, todos antes de los hijos existentes:

```js
lista.prepend(a, b, c);
// Resultado: a b c [hijos previos...]
// No al revés
```

## Recetas comunes

### Añadir un elemento de cabecera dinámico al inicio de una lista

```js
function insertarEncabezado(lista, texto) {
  const encabezado = document.createElement("li");
  encabezado.textContent = texto;
  encabezado.classList.add("lista__titulo");
  encabezado.setAttribute("aria-label", "Título de lista");
  lista.prepend(encabezado);
}

insertarEncabezado(document.querySelector(".tareas"), "Tareas pendientes");
```

### Mostrar el mensaje más reciente al principio (chat, feed)

```js
function agregarMensaje(contenedor, texto, autor) {
  const mensaje = document.createElement("div");
  mensaje.classList.add("mensaje");
  mensaje.innerHTML = `<strong>${autor}:</strong> `; // autor de confianza
  mensaje.append(document.createTextNode(texto));    // texto del usuario — escapado
  contenedor.prepend(mensaje);
}
```

### Añadir un nodo de aviso antes del contenido existente

```js
const aviso = document.createElement("p");
aviso.className = "aviso aviso--error";
aviso.textContent = "Hay errores en el formulario.";
formulario.prepend(aviso);
```

## Cómo funciona por dentro

`prepend` es un método de la interfaz `ParentNode` (disponible en `Element`, `Document` y `DocumentFragment`). Internamente convierte los strings a nodos de texto y luego llama a `insertBefore` repetidamente para cada argumento, usando el primer hijo original como nodo de referencia fija. El resultado es equivalente a un `insertBefore` por cada argumento, pero en una sola operación de la API pública.

Al igual que `append`, si alguno de los nodos pasados ya tiene un padre en el árbol, el motor lo desconecta de su padre actual antes de insertarlo en la nueva posición.

> [!tip]
> `prepend` es ideal para feeds, historiales, listas de notificaciones o cualquier interfaz donde los elementos nuevos aparecen al principio. Es la contraparte directa de `append` para inserción al inicio.

> [!warning]
> Insertar frecuentemente al inicio de un contenedor con muchos hijos puede ser costoso en términos de layout, porque el navegador necesita recalcular la posición de todos los hijos desplazados. Para listas largas en rendimiento crítico, considera invertir el orden de inserción (`append` al final) y usar CSS `flex-direction: column-reverse` para la presentación visual.

## Notas relacionadas

- [[02 appendChild y append]]
- [[04 insertBefore]]
- [[05 before y after]]
- [[index|Crear e Insertar Elementos — Índice]]
