---
title: Opciones de addEventListener — once, passive, capture, signal
aliases:
  - Opciones addEventListener
  - once passive capture
  - AddEventListenerOptions
tags:
  - javascript
  - api/metodo
  - eventos
objeto: EventTarget
tipo: metodo
retorna: undefined
muta: false
asincrono: false
draft: false
order: 2
---

# Opciones de addEventListener (once, passive, capture)

> [!definicion]
> El tercer argumento de `addEventListener` acepta un objeto de opciones o un booleano legacy. El objeto de opciones controla: en qué fase del evento se activa el listener (`capture`), si se elimina automáticamente tras la primera ejecución (`once`), si el handler promete no llamar `preventDefault` (`passive`), y si debe eliminarse cuando un `AbortSignal` se aborta (`signal`).

```js
btn.addEventListener('click', handler, {
  capture: false,   // burbujeo (default)
  once: true,       // se llama una vez y se auto-elimina
  passive: true,    // no llamará preventDefault()
});
```

## Tabla de opciones

| Opción | Tipo | Default | Efecto |
|---|---|---|---|
| `capture` | boolean | `false` | Activa el listener en fase de captura en lugar de burbujeo |
| `once` | boolean | `false` | El listener se invoca una sola vez y luego se elimina automáticamente |
| `passive` | boolean | `false` (o `true` en algunos eventos de scroll) | El handler promete no llamar `preventDefault()`; el navegador puede optimizar el scroll |
| `signal` | AbortSignal | `undefined` | El listener se elimina cuando el signal se aborta |

## Booleano legacy

Pasar `true` o `false` directamente como tercer argumento equivale a `{ capture: true }` o `{ capture: false }`. Esta forma existe por compatibilidad histórica; preferir el objeto de opciones para mayor claridad.

## `once: true`

El listener se ejecuta la primera vez que ocurre el evento y luego el motor lo elimina automáticamente. Equivale a llamar `removeEventListener` manualmente dentro del handler.

```js
// Mostrar splash solo la primera vez que el usuario hace clic
document.addEventListener('click', () => {
  mostrarBienvenida();
}, { once: true });
```

## `passive: true`

Indica al navegador que el handler no llamará `preventDefault()`. El navegador puede entonces iniciar el scroll inmediatamente sin esperar a que el JS termine — lo que elimina el jank en scroll táctil. Para `touchstart`, `touchmove`, `wheel` y `scroll`, la mayoría de los navegadores ya aplica `passive: true` por defecto en el documento raíz; en elementos anidados hay que indicarlo explícitamente.

```js
// Scroll fluido en lista larga
lista.addEventListener('touchmove', (e) => {
  actualizarIndicador(e.touches[0].clientY);
  // NO llamar e.preventDefault() aquí
}, { passive: true });
```

> [!warning]
> Si `passive: true` y el handler llama a `e.preventDefault()`, el navegador ignora la llamada (o lanza una advertencia en consola). No combinar `passive: true` con lógica que necesite cancelar el comportamiento por defecto del scroll o el touch.

## `signal: AbortSignal`

Permite eliminar varios listeners con un solo `controller.abort()`, sin necesidad de guardar referencias de función ni llamar `removeEventListener` por separado.

```js
const controller = new AbortController();
const { signal } = controller;

document.addEventListener('keydown', onKeydown, { signal });
document.addEventListener('click', onOutsideClick, { signal });
modal.addEventListener('submit', onSubmit, { signal });

// Al cerrar el modal, eliminar todos de una vez
function cerrarModal() {
  controller.abort();
  modal.remove();
}
```

> [!tip]
> `AbortController` + `signal` es la forma más ergonómica de gestionar el ciclo de vida de listeners. Un solo `controller.abort()` limpia todos los listeners registrados con ese `signal`, independientemente de cuántos sean o en qué elemento vivan.

## Notas relacionadas

- [[01 addEventListener|addEventListener]]
- [[03 removeEventListener|removeEventListener]]
- [[04 Fases del Evento/index|Fases del Evento]]
