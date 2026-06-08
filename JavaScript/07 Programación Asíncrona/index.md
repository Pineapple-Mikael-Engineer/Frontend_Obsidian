---
title: Programación Asíncrona en JavaScript
aliases:
  - asincronia javascript
  - async javascript
  - programacion asincrona js
tags:
  - javascript
  - api/concepto
  - asincrono
draft: false
---

# Programación Asíncrona

> [!definicion]
> La **programación asíncrona** en JavaScript es el conjunto de mecanismos que permiten al motor single-threaded iniciar operaciones lentas (I/O de red, timers, lectura de archivos) sin bloquear el hilo de ejecución, y recibir su resultado más tarde a través del Event Loop. La interfaz de usuario permanece responsiva mientras las operaciones pendientes se resuelven en el entorno del navegador o de Node.js.

El modelo evolucionó en tres generaciones: **callbacks** (delegación con inversión de control) → **Promesas** (valor futuro encadenable) → **async/await** (forma síncrona sobre Promesas). Los Workers añaden una cuarta dimensión: ejecutar código en paralelo real.

```js
// Las tres generaciones sobre la misma operación

// 1. Callback
fs.readFile("datos.json", (err, data) => { if (err) throw err; procesar(data); });

// 2. Promise
fetch("/api/datos").then(r => r.json()).then(procesar).catch(console.error);

// 3. async/await
async function cargar() {
  const datos = await fetch("/api/datos").then(r => r.json());
  procesar(datos);
}
```

## Subsecciones

| # | Nombre | Contenido principal |
|---|---|---|
| 01 | Síncrono vs Asíncrono | Modelo single-threaded, bloqueante vs no bloqueante, por qué JS necesita asincronía |
| 02 | Event Loop | Call Stack, Web APIs, Task Queue (macrotasks), Microtask Queue, orden de ejecución |
| 03 | Temporizadores | `setTimeout`, `setInterval`, `requestAnimationFrame`, precisión y drift |
| 04 | Callbacks | Patrón básico, callback hell, error-first pattern, inversión de control |
| 05 | Promesas | Estados, `then/catch/finally`, chaining, `Promise.all/allSettled/race/any` |
| 06 | Async / Await | Función `async`, `await`, manejo de errores con `try/catch`, paralelismo, `for await...of` |
| 07 | Tareas en Segundo Plano | Web Workers (cómputo intensivo), Service Workers (proxy de red, PWA, caché offline) |

## El modelo de ejecución

Todo el código JS asíncrono pasa por el Event Loop: las operaciones se delegan al entorno (navegador/Node), y sus callbacks regresan al hilo principal encolados como macrotasks o microtasks. Las microtasks (`.then`, `await`, `queueMicrotask`) se drenan completamente antes de que el Event Loop procese la siguiente macrotask (timer, evento, mensaje de Worker).

```
Call Stack vacío
      ↓
Drena Microtask Queue (Promise callbacks, await reanudaciones)
      ↓
Rendering (si corresponde al frame)
      ↓
Siguiente macrotask (setTimeout, setInterval, fetch response, message)
      ↓
(repite)
```

## Notas relacionadas

- [[01 Síncrono vs Asíncrono/index|Síncrono vs Asíncrono]]
- [[02 Event Loop/index|Event Loop]]
- [[03 Temporizadores/index|Temporizadores]]
- [[04 Callbacks/index|Callbacks]]
- [[05 Promesas/index|Promesas]]
- [[06 Async - Await/index|Async / Await]]
- [[07 Tareas en Segundo Plano/index|Tareas en Segundo Plano]]
