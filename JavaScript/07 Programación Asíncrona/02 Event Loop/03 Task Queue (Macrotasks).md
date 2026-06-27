---
title: Task Queue (Macrotasks) — Cola de tareas del Event Loop
aliases:
  - task queue
  - macrotask queue
  - message queue
  - macrotasks
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
order: 3
---

# Task Queue (Macrotasks)

> [!definicion]
> La **Task Queue** (también llamada macrotask queue o message queue) es la cola donde el entorno deposita callbacks listos para ejecutarse: callbacks de `setTimeout`/`setInterval`, eventos del DOM, callbacks de I/O. El Event Loop extrae **una sola macrotask por iteración**, la ejecuta hasta que el Call Stack queda vacío, y solo entonces vacía las microtasks antes de tomar la siguiente.

```js
setTimeout(() => console.log("macro 1"), 0);
setTimeout(() => console.log("macro 2"), 0);
Promise.resolve().then(() => console.log("micro"));

// Output: micro → macro 1 → macro 2
// Explicación:
// iteración 1: stack vacío → vacía microtasks ("micro") → toma macro 1
// iteración 2: stack vacío → vacía microtasks (vacía) → toma macro 2
```

## Fuentes de macrotasks

| Origen | Descripción |
|---|---|
| `setTimeout(fn, ms)` | Callback encolado cuando el timer expira |
| `setInterval(fn, ms)` | Callback encolado cada `ms` milisegundos |
| `setImmediate(fn)` | Node.js: encolado al final de la iteración actual del loop de I/O |
| `MessageChannel` | Comunicación entre contextos (Web Workers, iframes) |
| Eventos del DOM | `click`, `keydown`, `load`, `scroll` — cada evento es una macrotask |
| I/O callbacks | Callbacks de red, disco en Node.js (via libuv) |
| `IndexedDB` callbacks | Respuestas de base de datos local |

## Una macrotask por iteración — por qué importa

El diseño "una macrotask por iteración" es deliberado: permite al browser intercalar el paso de render entre macrotasks. Si el Event Loop ejecutara todas las macrotasks pendientes antes de renderizar, una ráfaga de eventos (como `mousemove` a 60 Hz) bloquearía el render.

```js
// Ejemplo: diferir trabajo pesado en trozos con setTimeout
function procesarEnTrozos(items) {
  const TROZO = 100;
  let i = 0;

  function siguiente() {
    const fin = Math.min(i + TROZO, items.length);
    for (; i < fin; i++) procesar(items[i]);

    if (i < items.length) {
      setTimeout(siguiente, 0); // cede el hilo tras cada trozo
    }
  }

  setTimeout(siguiente, 0);
}
// Cada setTimeout encola una nueva macrotask,
// permitiendo al browser renderizar entre trozos
```

## `setTimeout(fn, 0)` — no es inmediato

`setTimeout` con delay `0` no ejecuta el callback en la misma iteración del loop. El callback se encola en la Task Queue y el Event Loop lo procesa solo después de vaciar las microtasks de la iteración actual:

```js
console.log("1 sync");

setTimeout(() => console.log("3 macro"), 0);

Promise.resolve()
  .then(() => console.log("2a micro"))
  .then(() => console.log("2b micro")); // microtask generada por microtask

// Output: 1 sync → 2a micro → 2b micro → 3 macro
```

El delay especificado en `setTimeout` es un **mínimo**, no un exacto. Si el hilo está ocupado con una long task, el callback puede retrasarse significativamente.

## Cómo funciona por dentro

La Task Queue es una estructura FIFO (First In, First Out). El entorno (browser o libuv) deposita callbacks en la cola; el Event Loop los consume de uno en uno. En pseudocódigo:

```text
while (true) {
  // 1. Ejecutar script actual (ya está en el call stack)

  // 2. Vaciar microtasks
  while (microtaskQueue.length > 0) {
    const micro = microtaskQueue.shift();
    micro();
  }

  // 3. Render si aplica

  // 4. Tomar una macrotask
  if (taskQueue.length > 0) {
    const task = taskQueue.shift();
    task(); // pone frames en el call stack
  }
}
```

> [!tip]
> `MessageChannel` es una alternativa a `setTimeout(fn, 0)` para encolar macrotasks con menor overhead, ya que no tiene el mínimo de 4 ms impuesto por el estándar HTML a timers anidados. Se usa en implementaciones de schedulers como React's scheduler.

> [!warning]
> No hay garantía de orden entre macrotasks de distinta fuente. Si un `setTimeout(fn, 0)` y un evento de click quedan listos al mismo tiempo, el orden en que el browser los encola depende de la implementación. Sí hay garantía de que dos `setTimeout(fn, 0)` consecutivos se despachan en el orden en que fueron registrados.

## Notas relacionadas

- [[index|Event Loop]]
- [[04 Microtask Queue|Microtask Queue]]
- [[05 Orden de Ejecución (micro > macro)|Orden de Ejecución (micro > macro)]]
- [[02 Web APIs|Web APIs]]
