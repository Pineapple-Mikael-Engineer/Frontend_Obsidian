---
title: "before y after"
aliases:
  - nodo.before
  - nodo.after
  - insertar hermano DOM
tags:
  - javascript
  - api/metodo
  - dom
draft: false
order: 5
---

# `before` y `after`

> [!definicion]
> `nodo.before(...nodosOStrings)` inserta los nodos y strings proporcionados como **hermanos anteriores** del nodo, justo antes de él en el árbol. `nodo.after(...nodosOStrings)` los inserta como **hermanos siguientes**, justo después. Ambos se invocan sobre el nodo de referencia directamente — no requieren referencia al padre. Devuelven `undefined`.

```js
const parrafo = document.querySelector("p.central");

// Insertar antes
const titulo = document.createElement("h2");
titulo.textContent = "Sección nueva";
parrafo.before(titulo);
// Resultado: <h2>Sección nueva</h2> <p class="central">...</p>

// Insertar después
const nota = document.createElement("aside");
nota.textContent = "Nota al pie";
parrafo.after(nota);
// Resultado: <p class="central">...</p> <aside>Nota al pie</aside>
```

## Múltiples argumentos

Igual que `append` y `prepend`, ambos métodos aceptan múltiples nodos y strings en una sola llamada, insertados en el orden en que se pasan:

```js
const el = document.querySelector(".objetivo");
el.before("Prefijo: ", span, " — ");
// Resultado: Prefijo:  <span>...</span>  —  [.objetivo]

el.after(separador, parrafo, "Fin");
// Resultado: [.objetivo] [separador] [parrafo] Fin
```

## Comparativa con `insertBefore`

| Aspecto | `padre.insertBefore(nuevo, ref)` | `ref.before(...items)` |
|---|---|---|
| Se invoca sobre | El padre | El nodo de referencia |
| Requiere referencia al padre | Sí | No |
| Inserta múltiples items | No | Sí |
| Acepta strings | No | Sí |
| Devuelve | El nodo insertado | `undefined` |
| API | Clásica | Moderna |
| Equivalente de `after` | No tiene directo | `ref.after(...)` |

## Recetas comunes

### Insertar un label antes de un input sin conocer el padre

```js
const input = document.querySelector("#email");
const label = document.createElement("label");
label.htmlFor = "email";
label.textContent = "Correo electrónico";
input.before(label);
// No necesitamos input.parentNode
```

### Insertar un mensaje de éxito después de un formulario

```js
function mostrarExito(formulario) {
  const mensaje = document.createElement("div");
  mensaje.className = "alerta alerta--exito";
  mensaje.role = "alert";
  mensaje.textContent = "Formulario enviado correctamente.";
  formulario.after(mensaje);
  setTimeout(() => mensaje.remove(), 4000);
}
```

### Insertar separadores entre elementos hermanos

```js
const items = [...document.querySelectorAll(".nav__item")];
// Insertar un separador después de cada item excepto el último
items.slice(0, -1).forEach(item => {
  const sep = document.createElement("span");
  sep.className = "nav__separador";
  sep.setAttribute("aria-hidden", "true");
  item.after(sep);
});
```

### Envolver un elemento con un contenedor (wrapper pattern)

```js
function envolver(el, tipoContenedor) {
  const wrapper = document.createElement(tipoContenedor);
  el.before(wrapper);   // insertar el wrapper en la posición del elemento
  wrapper.appendChild(el); // mover el elemento dentro del wrapper
  return wrapper;
}

const img = document.querySelector("img.destacada");
envolver(img, "figure");
```

## Cómo funciona por dentro

`before` y `after` son métodos de la interfaz `ChildNode` (disponible en `Element`, `CharacterData` y `DocumentType`). Internamente, ambos obtienen `this.parentNode` (el padre del nodo sobre el que se invocan) y luego realizan las inserciones usando la lógica equivalente a `insertBefore` del padre. Si `this.parentNode` es `null` (el nodo no está en el árbol), los métodos no hacen nada y no lanzan error — la operación se descarta silenciosamente.

Los strings pasados como argumentos se convierten a nodos de texto con escape automático antes de la inserción.

> [!tip]
> `before` y `after` son la forma moderna y preferida de insertar hermanos. Eliminan la necesidad de acceder a `parentNode` explícitamente, haciendo el código más robusto ante cambios en la estructura del árbol — si el padre cambia, el código sigue funcionando siempre que el nodo de referencia esté en el DOM.

> [!warning]
> Si el nodo sobre el que se invoca `before` o `after` no tiene padre (no está conectado al DOM), la llamada se ignora silenciosamente sin lanzar ningún error. Si el nodo no aparece en el lugar esperado después de la operación, verifica que `nodo.parentNode !== null` antes de llamar al método.

## Notas relacionadas

- [[04 insertBefore]]
- [[03 prepend]]
- [[02 appendChild y append]]
- [[index|Crear e Insertar Elementos — Índice]]
