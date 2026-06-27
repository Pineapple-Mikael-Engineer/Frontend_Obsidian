---
title: Microtask Queue — Cola de alta prioridad del Event Loop
aliases:
  - microtask queue
  - microtasks
  - cola de microtareas
tags:
  - javascript
  - api/concepto
  - asincrono
objeto: global
tipo: concepto
retorna: undefined
muta: false
asincrono: false
draft: false
order: 4
---

# Microtask Queue

> [!definicion]
> La **Microtask Queue** es una cola de alta prioridad que el Event Loop vacía **completamente** después de cada tarea y al final de cada script, antes de pasar a la siguiente macrotask o al paso de render. Las microtasks generadas durante el vaciado de la cola se procesan en la misma pasada.

```js
console.log("1 sync");

Promise.resolve()
  .then(() => {
    console.log("2 micro");
    // Esta microtask genera otra microtask:
    return Promise.resolve();
  })
  .then(() => console.log("3 micro (generada por micro)"));

setTimeout(() => console.log("4 macro"), 0);

console.log("5 sync");

// Output: 1 sync → 5 sync → 2 micro → 3 micro (generada por micro) → 4 macro
```

La microtask generada dentro de otra microtask (`3 micro`) se procesa en la misma pasada, antes del `setTimeout`.

## Fuentes de microtasks

| Fuente | API |
|---|---|
| Resolución de Promise | `.then()`, `.catch()`, `.finally()` |
| `queueMicrotask(fn)` | API explícita para encolar microtasks |
| `MutationObserver` | Cambios en el árbol del DOM |
| `async/await` | Cada `await` genera microtasks internamente |

`async/await` compila a Promises internamente — el código tras un `await` es un callback `.then()` encolado como microtask:

```js
async function ejemplo() {
  console.log("A sync");        // síncrono
  await Promise.resolve();      // suspende la función
  console.log("B micro");       // microtask — corre después de C
}

ejemplo();
console.log("C sync");

// Output: A sync → C sync → B micro
```

## Prioridad completa — vaciado exhaustivo

La distinción clave respecto a la Task Queue: el Event Loop vacía la Microtask Queue **hasta que esté vacía**, incluyendo microtasks añadidas durante el proceso. Solo entonces pasa a la siguiente macrotask:

```js
// Ciclo de microtasks — peligroso
let i = 0;
function microtaskLoop() {
  if (i++ < 1_000_000) {
    queueMicrotask(microtaskLoop); // encola otra microtask
  }
}
queueMicrotask(microtaskLoop);

setTimeout(() => console.log("macro"), 0);
// "macro" no aparece hasta que se completen los 1.000.000 de microtasks
```

## `queueMicrotask` — uso explícito

`queueMicrotask(fn)` encola `fn` directamente en la Microtask Queue sin crear una Promise. Útil cuando se necesita diferir trabajo brevemente pero con prioridad sobre cualquier `setTimeout(fn, 0)`:

```js
queueMicrotask(() => console.log("micro"));
setTimeout(() => console.log("macro"), 0);

// Output: micro → macro
// Aunque ambos se registraron "ahora", micro tiene prioridad
```

## Cómo funciona por dentro

El motor JS mantiene la Microtask Queue separada de la Task Queue. En V8, se denomina internamente "PromiseJobs queue" (herencia de la spec ECMAScript que llama a las microtasks "PromiseJobs"). El algoritmo de vaciado:

```text
function vaciarMicrotasks() {
  while (microtaskQueue.length > 0) {
    const micro = microtaskQueue.shift();
    micro();
    // Si micro() encola más microtasks, se procesan en este mismo bucle
  }
}
```

Este vaciado ocurre:
- Al final del script principal (antes del primer evento).
- Después de cada macrotask.
- Después de cada callback de `requestAnimationFrame` (antes del render en algunos browsers).

> [!tip]
> Para diferir trabajo que debe ejecutarse "lo antes posible" pero sin bloquear el render, `queueMicrotask` es más predecible que `Promise.resolve().then(fn)` porque no crea el overhead de una Promise y su semántica es explícita. Para diferir hasta el siguiente frame visual, usar `requestAnimationFrame`.

> [!warning]
> Una cadena de microtasks que se regenera indefinidamente (`queueMicrotask` llamando a `queueMicrotask` en bucle sin condición de corte) impide que el Event Loop avance a la siguiente macrotask y bloquea el render de la UI, aunque el Call Stack esté vacío. El síntoma es idéntico a un bucle infinito síncrono: la página deja de responder.

## Notas relacionadas

- [[index|Event Loop]]
- [[03 Task Queue (Macrotasks)|Task Queue (Macrotasks)]]
- [[05 Orden de Ejecución (micro > macro)|Orden de Ejecución (micro > macro)]]
- [[01 Call Stack|Call Stack]]
