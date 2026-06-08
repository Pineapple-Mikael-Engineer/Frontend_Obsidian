---
title: Orden de Ejecución — micro > macro en el Event Loop
aliases:
  - orden event loop
  - micro macro orden
  - prioridad microtask macrotask
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
---

# Orden de Ejecución (micro > macro)

> [!definicion]
> El Event Loop sigue un orden de prioridad estricto: **síncrono → microtasks → render → macrotask**. Primero agota el script en ejecución, luego vacía completamente la Microtask Queue, luego permite al browser renderizar si es necesario, y finalmente toma una macrotask. El ciclo se repite.

```js
console.log("1 sync");
setTimeout(() => console.log("4 macro"), 0);
Promise.resolve().then(() => console.log("3 micro"));
console.log("2 sync");
// Output: 1 sync → 2 sync → 3 micro → 4 macro
```

La Promise se resuelve "de forma síncrona" (ya tiene valor), pero su callback `.then()` siempre es asíncrono — se encola en la Microtask Queue y corre después de que el script síncrono termine.

## El ciclo completo

```text
┌─────────────────────────────────────────────────┐
│  1. Ejecutar script síncrono actual             │
│     (Call Stack hasta vaciarse)                 │
└───────────────────┬─────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────┐
│  2. Vaciar Microtask Queue completa             │
│     (incluyendo microtasks generadas por micro) │
└───────────────────┬─────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────┐
│  3. Render (si el browser lo considera, ~16ms)  │
│     requestAnimationFrame callbacks aquí        │
└───────────────────┬─────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────┐
│  4. Tomar UNA macrotask de la Task Queue        │
└───────────────────┬─────────────────────────────┘
                    ↓
                 volver a 2
```

## Ejemplo completo con todas las colas

```js
console.log("A");                                      // síncrono

setTimeout(() => console.log("E macro 1"), 0);        // macrotask

Promise.resolve()
  .then(() => {
    console.log("C micro 1");                          // microtask
    return Promise.resolve();
  })
  .then(() => console.log("D micro 2"));               // microtask (generada)

setTimeout(() => console.log("F macro 2"), 0);        // macrotask

console.log("B");                                      // síncrono

// Output: A → B → C micro 1 → D micro 2 → E macro 1 → F macro 2
```

Análisis iteración a iteración:
1. Script: `A`, registra `E`, registra `C`, registra `F`, imprime `B`. Stack vacío.
2. Microtasks: `C micro 1` (que encola `D micro 2`), luego `D micro 2`. Cola vacía.
3. Macrotask: `E macro 1`. Stack vacío.
4. Microtasks: vacía (ninguna nueva). Macrotask: `F macro 2`.

## `async/await` — síncrono hasta el primer `await`

```js
async function tarea() {
  console.log("X sync");          // síncrono — antes del await
  await Promise.resolve();        // suspende; lo de abajo es una microtask
  console.log("Z micro");         // microtask
}

tarea();
console.log("Y sync");

// Output: X sync → Y sync → Z micro
```

El código hasta el primer `await` corre sincrónicamente en la iteración actual. Tras el `await`, el resto de la función se convierte en el callback de una microtask, con el mismo comportamiento que `.then()`.

Cuando hay múltiples `await` encadenados:

```js
async function encadenada() {
  await Promise.resolve();
  console.log("micro 1");     // microtask tras 1er await
  await Promise.resolve();
  console.log("micro 2");     // microtask tras 2do await
}

encadenada();
setTimeout(() => console.log("macro"), 0);

// Output: micro 1 → micro 2 → macro
// Ambos await generan microtasks, y se procesan antes del setTimeout
```

## Tabla de referencia — cuándo se ejecuta cada expresión

| Expresión | Tipo | Cuándo corre |
|---|---|---|
| `console.log("x")` | Síncrono | Inmediatamente, en orden |
| `Promise.resolve().then(fn)` | Microtask | Tras script actual, antes de macro |
| `queueMicrotask(fn)` | Microtask | Tras script actual, antes de macro |
| `MutationObserver` callback | Microtask | Tras script actual, antes de macro |
| `setTimeout(fn, 0)` | Macrotask | Próxima iteración del loop (tras micros) |
| `setInterval(fn, ms)` | Macrotask | Cada `ms` ms, una iteración por disparo |
| Evento DOM (`click`, etc.) | Macrotask | Cuando el evento ocurre, una por evento |
| `requestAnimationFrame(fn)` | Callback de animación | Antes del siguiente frame de render |
| `await expr` (código tras await) | Microtask | Tras resolución de `expr` |

## Cómo funciona por dentro — la spec

La especificación ECMAScript define el procesamiento de microtasks bajo el nombre "PerformPromiseThen" y "EnqueueJob". El HTML Living Standard define el Event Loop, incluyendo la integración con el render y la Task Queue. La invariante central: **la Microtask Queue se vacía en cada "checkpoint"** — tras cada tarea, y tras cada callback de la API de render.

> [!tip]
> Para predecir el orden de ejecución de cualquier código asíncrono: identificar cada expresión como síncrona / microtask / macrotask, aplicar el ciclo del loop, y razonar iteración a iteración. El 95% de los puzzles de entrevista sobre Event Loop se resuelven con este algoritmo mecánico.

> [!warning]
> El orden entre microtasks de distintas Promises que se resuelven en la misma iteración depende del orden en que fueron registradas con `.then()`, no del orden en que las Promises se resolvieron. Una Promise que ya estaba resuelta cuando se llama `.then()` encola su callback inmediatamente — no espera.

## Notas relacionadas

- [[index|Event Loop]]
- [[04 Microtask Queue|Microtask Queue]]
- [[03 Task Queue (Macrotasks)|Task Queue (Macrotasks)]]
- [[01 Call Stack|Call Stack]]
- [[02 Web APIs|Web APIs]]
