---
title: removeEventListener — Eliminar listeners registrados
aliases:
  - removeEventListener
  - EventTarget.removeEventListener
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
order: 3
---

# removeEventListener

> [!definicion]
> `elemento.removeEventListener(tipo, handler, opcionesOCaptura?)` elimina un listener previamente registrado. Para que la eliminación surta efecto, los tres argumentos deben coincidir exactamente con los del `addEventListener` original: mismo tipo de evento, misma referencia de función, y misma fase (captura o burbujeo).

```js
function alHacerClic(e) {
  console.log('clic', e.target);
}

btn.addEventListener('click', alHacerClic);
// ... más tarde:
btn.removeEventListener('click', alHacerClic);
// → el listener ya no se ejecuta
```

## La trampa del listener anónimo

La causa más común de que `removeEventListener` no funcione: pasar una función anónima o arrow inline a `addEventListener`. Cada llamada crea una nueva referencia de función distinta; sin la referencia original, no hay forma de eliminar el listener.

```js
// MAL — no se puede eliminar
btn.addEventListener('click', (e) => console.log(e.target));
btn.removeEventListener('click', (e) => console.log(e.target));
// → no elimina nada; son dos funciones distintas en memoria

// BIEN — referencia nombrada
const handler = (e) => console.log(e.target);
btn.addEventListener('click', handler);
btn.removeEventListener('click', handler);  // elimina correctamente
```

## Coincidencia de fase

Un listener registrado con `capture: true` debe eliminarse con `capture: true`. Pasar `capture: false` (o sin tercer argumento) no elimina un listener de captura.

```js
btn.addEventListener('click', handler, { capture: true });

btn.removeEventListener('click', handler);              // ❌ no elimina (fase distinta)
btn.removeEventListener('click', handler, true);         // ✅
btn.removeEventListener('click', handler, { capture: true }); // ✅
```

## Listener de un modal — receta

```js
function abrirModal(modal) {
  const controller = new AbortController();

  function cerrar() {
    modal.remove();
    controller.abort();  // limpia todos los listeners del modal
  }

  modal.querySelector('.cerrar').addEventListener('click', cerrar, { signal: controller.signal });
  document.addEventListener('keydown', (e) => {
    if (e.key === 'Escape') cerrar();
  }, { signal: controller.signal });

  document.body.appendChild(modal);
}
```

## Alternativa moderna: `AbortController`

Para múltiples listeners relacionados, `AbortController` + `signal` es más ergonómico que guardar referencias y llamar `removeEventListener` individualmente. Se delega el detalle al nodo [[02 Opciones (once, passive, capture)|Opciones de addEventListener]].

```js
const controller = new AbortController();

el.addEventListener('mousedown', onStart, { signal: controller.signal });
el.addEventListener('mousemove', onMove, { signal: controller.signal });
el.addEventListener('mouseup', onEnd, { signal: controller.signal });

// Eliminar todos con una sola llamada
controller.abort();
```

> [!tip]
> Siempre que un componente se destruya o desmonte, limpiar sus listeners. Los listeners que quedan después de que un elemento se elimina del DOM pueden provocar memory leaks si mantienen referencias a objetos grandes en el closure del handler.

> [!warning]
> `removeEventListener` no lanza error si el listener no existe o los argumentos no coinciden: simplemente no hace nada. Un argumento de fase incorrecto es difícil de depurar porque la llamada parece correcta pero el listener sigue activo.

## Notas relacionadas

- [[01 addEventListener|addEventListener]]
- [[02 Opciones (once, passive, capture)|Opciones (once, passive, capture)]]
