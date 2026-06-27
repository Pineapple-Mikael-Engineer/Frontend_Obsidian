---
title: Single-Threaded — Un solo hilo de ejecución en JavaScript
aliases:
  - single thread js
  - hilo unico javascript
  - un solo hilo
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
order: 1
---

# Single-Threaded

> [!definicion]
> JavaScript corre en un **único hilo de ejecución**: hay un solo Call Stack y un solo flujo de control. Dos fragmentos de código de usuario nunca se ejecutan simultáneamente en el mismo contexto JS.

El hilo único simplifica el modelo de concurrencia: no hay race conditions en el estado compartido del heap porque ningún otro hilo puede modificar variables mientras el código JS se ejecuta. La contrapartida es que **código bloqueante congela toda la aplicación**.

## Consecuencias directas

```js
// Este while bloquea el hilo — la UI se congela completamente
while (true) {
  // nada más puede correr: ni eventos, ni timers, ni render
}

// JSON.parse de un payload enorme — bloquea el hilo durante ms o segundos
const obj = JSON.parse(hugeMegabyteString);

// alert() — bloquea hasta que el usuario acepta
alert("El hilo está parado aquí");
```

Cualquier operación que mantenga el Call Stack ocupado durante más de ~50 ms se considera una **long task** y degrada la experiencia de usuario de forma perceptible.

## Por qué otros hilos no resuelven directamente el problema de JS

Los navegadores y Node.js sí usan hilos adicionales internamente:

| Entorno | Hilos auxiliares |
|---|---|
| Navegador | Web Workers (JS aislado), hilos de red, GPU, renderer |
| Node.js | libuv thread pool (I/O de disco, crypto, DNS) |

La diferencia clave: esos hilos **no comparten el heap** con el código JS del usuario (Web Workers tienen su propio contexto aislado; libuv opera en C). El código JS del usuario siempre corre en el thread principal. La comunicación con Web Workers ocurre por mensajes (`postMessage`), no por memoria compartida (salvo `SharedArrayBuffer` con `Atomics`, que es opt-in explícito).

## Cómo funciona por dentro

El motor V8 (y cualquier motor JS) mantiene:

1. **Call Stack** — pila de frames de función, evaluada secuencialmente.
2. **Heap** — zona de memoria para objetos y closures.

No hay scheduler de hilos para código de usuario. La concurrencia aparente viene del [[02 Event Loop/index|Event Loop]]: cuando el stack está vacío, el loop despacha el siguiente callback. Para el código, parece que las cosas "ocurren en paralelo", pero en realidad se intercalan entre iteraciones del loop.

## Detectar código bloqueante

El Performance panel de Chrome DevTools clasifica tareas de más de 50 ms como **Long Tasks** (marcadas en rojo). El patrón para diagnosticar:

```js
// Medir cuánto bloquea una operación
const t0 = performance.now();
heavyComputation();
const t1 = performance.now();
console.log(`Bloqueó el hilo ${(t1 - t0).toFixed(2)} ms`);
```

Para trabajo computacional intenso que debe correr en el navegador sin bloquear: delegar a un **Web Worker**.

> [!tip]
> La regla de los 50 ms proviene del modelo RAIL de Google: para que la UI se sienta instantánea, cada frame debe procesarse en menos de 16 ms (60 fps), y una tarea individual no debería robar más de 50 ms del hilo principal antes de ceder el control al Event Loop.

> [!warning]
> `JSON.parse`, `JSON.stringify`, operaciones sobre arrays o strings muy grandes, y ciclos computacionales intensos son las fuentes más comunes de long tasks en aplicaciones reales. No confundir "ejecuta rápido en un array pequeño" con "seguro en producción con datos reales".

## Notas relacionadas

- [[02 Bloqueante vs No Bloqueante|Bloqueante vs No Bloqueante]]
- [[02 Event Loop/index|Event Loop]]
- [[02 Event Loop/01 Call Stack|Call Stack]]
