---
title: Web APIs — Operaciones asíncronas del entorno
aliases:
  - web apis
  - browser apis async
  - libuv
tags:
  - javascript
  - api/concepto
  - asincrono
objeto: Web API
tipo: concepto
retorna: undefined
muta: false
asincrono: true
draft: false
order: 2
---

# Web APIs

> [!definicion]
> Las **Web APIs** son funciones provistas por el entorno de ejecución (navegador o Node.js), no por el motor JS, que manejan operaciones que requieren esperar: timers, peticiones de red, lectura de disco, eventos del DOM. Cuando el código JS las invoca, el entorno registra la operación y devuelve el control inmediatamente; el hilo JS no espera. Cuando la operación termina, el entorno encola el callback en la Task Queue o la Microtask Queue.

```js
console.log("antes");

setTimeout(() => {
  console.log("timer — Task Queue");
}, 1000);

fetch("https://api.ejemplo.com/datos")
  .then(res => res.json())
  .then(data => console.log("fetch — Microtask Queue", data));

console.log("después");
// Output inmediato: "antes" → "después"
// Después de ~1s: "timer — Task Queue"
// Cuando llega la respuesta: "fetch — Microtask Queue ..."
```

## Tabla de Web APIs y cola de destino

| API | Evento que dispara | Cola destino |
|---|---|---|
| `setTimeout(fn, ms)` | Timer expira | Task Queue (macrotask) |
| `setInterval(fn, ms)` | Timer expira (cada vez) | Task Queue (macrotask) |
| `fetch` / `XMLHttpRequest` | Respuesta de red recibida | Microtask Queue (via Promise) |
| `addEventListener` | Evento del DOM disparado | Task Queue (macrotask) |
| `requestAnimationFrame` | Antes del siguiente frame de render | Callback de animación (prioridad propia) |
| `IntersectionObserver` | Elemento entra/sale del viewport | Task Queue (macrotask) |
| `MutationObserver` | Cambio en el DOM | Microtask Queue |
| `queueMicrotask(fn)` | Inmediato tras la tarea actual | Microtask Queue |
| `Promise` (resolve/reject) | Resolución/rechazo | Microtask Queue |
| `IndexedDB` | Operación completada | Task Queue (macrotask) |
| `WebSocket` (onmessage) | Mensaje recibido | Task Queue (macrotask) |

## Cómo funciona por dentro

Las Web APIs no corren código JS — corren en el entorno del navegador (en C++). El flujo para `setTimeout`:

1. JS llama a `setTimeout(fn, 1000)` — el motor registra la petición en el browser.
2. El browser inicia un timer en su propio scheduler (fuera del hilo JS).
3. El hilo JS sigue ejecutando el código siguiente. El stack no está bloqueado.
4. Cuando el timer expira (~1000 ms después), el browser toma `fn` y lo coloca en la Task Queue.
5. El [[index|Event Loop]] detecta el Call Stack vacío y despacha `fn` desde la Task Queue.

Para `fetch`, el flujo es el mismo pero la operación de red la maneja el motor de red del browser (o `http` module en Node). La resolución de la Promise encola el `.then()` en la Microtask Queue.

## Node.js — libuv como equivalente

En Node.js no hay Web APIs de browser. El rol lo cumple **libuv**, una biblioteca C que provee:

- **Thread pool** (4 hilos por defecto, configurable con `UV_THREADPOOL_SIZE`): operaciones de disco (`fs`), `crypto`, `dns.lookup`, `zlib`.
- **I/O asíncrono sin threads** (epoll/kqueue/IOCP según OS): sockets de red, pipes, signals.

```js
// Node — fs.readFile delega a libuv, que usa el thread pool
const fs = require("fs");
fs.readFile("archivo.txt", "utf8", (err, data) => {
  // Este callback corre en el hilo JS principal
  // cuando libuv termina la lectura en su thread pool
  console.log(data);
});
```

La API de alto nivel es idéntica al patrón de Web APIs: el código JS delega, el entorno trabaja fuera, el callback vuelve al hilo principal.

## Timers — garantías y límites

`setTimeout(fn, 0)` no ejecuta `fn` inmediatamente. El estándar HTML especifica un mínimo de 4 ms para timers anidados (profundidad > 5). En práctica:

```js
const t0 = performance.now();
setTimeout(() => {
  const delay = performance.now() - t0;
  console.log(`Delay real: ${delay.toFixed(2)} ms`); // típicamente 1–5 ms, nunca 0
}, 0);
```

El timer expira, el callback se encola en la Task Queue, y el Event Loop lo despacha solo cuando el Call Stack está vacío y las microtasks han sido procesadas. El retraso real siempre es mayor que el especificado.

> [!tip]
> `requestAnimationFrame` no usa la Task Queue estándar — tiene su propio momento de ejecución justo antes del paso de render del browser, lo que lo hace ideal para animaciones: garantiza que el cambio visual se aplica antes del próximo frame.

> [!warning]
> Las Web APIs no son parte de la especificación ECMAScript. `setTimeout` no existe en el spec del lenguaje — lo provee el entorno. En un entorno que no lo implemente (un motor JS embebido mínimo), no existe. Esto explica diferencias de comportamiento entre navegadores y versiones de Node.js.

## Notas relacionadas

- [[index|Event Loop]]
- [[01 Call Stack|Call Stack]]
- [[03 Task Queue (Macrotasks)|Task Queue (Macrotasks)]]
- [[04 Microtask Queue|Microtask Queue]]
