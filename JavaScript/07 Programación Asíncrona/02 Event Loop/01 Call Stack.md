---
title: Call Stack — Pila de ejecución de JavaScript
aliases:
  - call stack
  - pila de llamadas
  - stack de ejecucion
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

# Call Stack

> [!definicion]
> El **Call Stack** es una estructura de datos LIFO (Last In, First Out) que registra qué función se está ejecutando y desde dónde fue llamada. Cada invocación de función añade un **frame** al tope del stack con sus variables locales y el punto de retorno. Cuando la función retorna, su frame se elimina.

```js
function c() { return 1; }
function b() { return c(); }
function a() { return b(); }

a();
// Secuencia del stack:
// [a]         → a invoca b
// [a, b]      → b invoca c
// [a, b, c]   → c retorna 1, frame c se elimina
// [a, b]      → b retorna 1, frame b se elimina
// [a]         → a retorna 1, frame a se elimina
// []          → stack vacío, Event Loop puede actuar
```

## Estructura de un frame

Cada frame almacena:
- Referencia a la función en ejecución.
- Variables locales y parámetros.
- El punto de retorno (dirección de la instrucción siguiente en la función llamante).

V8 y los demás motores JS implementan el Call Stack como un array de frames con un puntero al tope. El tamaño máximo es finito: en V8 aproximadamente 10 000–15 000 frames dependiendo del tamaño de cada frame.

## Stack overflow — desbordamiento

```js
function infinita() {
  return infinita(); // no hay caso base
}

infinita();
// → RangeError: Maximum call stack size exceeded
```

La excepción `RangeError: Maximum call stack size exceeded` (V8) o `InternalError: too much recursion` (Firefox) se lanza cuando el stack supera su límite. No hay forma de atrapar el stack overflow de forma fiable porque el propio `try/catch` requiere frames adicionales.

La recursión legítima profunda se transforma en iterativa o se implementa con trampolining para evitar el desbordamiento:

```js
// Recursivo — puede desbordar con n grande
function factorial(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}

// Iterativo — stack constante
function factorialIter(n) {
  let acc = 1;
  for (let i = 2; i <= n; i++) acc *= i;
  return acc;
}
```

## Relación con el Event Loop

**El Event Loop solo puede despachar un callback cuando el Call Stack está completamente vacío.** Mientras haya frames en el stack, ningún callback de la Task Queue ni de la Microtask Queue puede ejecutarse. Esto garantiza que el código en ejecución no sea interrumpido por eventos externos — la atomicidad de cada tarea.

```js
setTimeout(() => console.log("timer"), 0);

// Este bucle mantiene el stack ocupado durante ~3s
const limite = Date.now() + 3000;
while (Date.now() < limite) {}

// Solo cuando el while termina (stack vacío), el Event Loop despacha el timer
console.log("bucle terminado");
// Output: "bucle terminado" → "timer"  (con ~3s de retraso)
```

## Stack trace y depuración

El stack trace de un error refleja exactamente el estado del Call Stack en el momento de la excepción:

```text
TypeError: Cannot read properties of undefined (reading 'id')
    at getUser (app.js:12:18)      ← frame superior (más reciente)
    at processRequest (app.js:28:5)
    at handleEvent (app.js:45:3)   ← frame inferior (más antiguo)
```

En código asíncrono con callbacks planos, el stack trace muestra solo el frame del callback y el del motor, no el sitio donde se originó la operación. Las Promises y `async/await` producen stack traces más informativos en V8 moderno gracias a la captura de async stack frames.

> [!tip]
> DevTools → Sources → "Async" activa los async stack frames, que muestran el contexto completo de una cadena de promesas o await aunque el código cruce múltiples microtasks.

> [!warning]
> Un Call Stack vacío no significa que no haya trabajo pendiente — puede haber callbacks esperando en la Microtask Queue o la Task Queue. El stack vacío es condición necesaria pero no suficiente para que el programa haya "terminado".

## Notas relacionadas

- [[index|Event Loop]]
- [[04 Microtask Queue|Microtask Queue]]
- [[03 Task Queue (Macrotasks)|Task Queue (Macrotasks)]]
- [[05 Orden de Ejecución (micro > macro)|Orden de Ejecución (micro > macro)]]
