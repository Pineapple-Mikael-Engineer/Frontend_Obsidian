---
title: setTimeout — retardo único en JavaScript
aliases:
  - setTimeout
  - clearTimeout
tags:
  - javascript
  - api/metodo
  - asincrono
draft: false
---

# setTimeout

> [!definicion]
> `setTimeout(fn, delay, ...args)` encola `fn` como una macrotask que se ejecutará después de al menos `delay` milisegundos. Devuelve un `timeoutId` (número entero) que puede usarse para cancelar el timeout con `clearTimeout(timeoutId)`.

```js
const id = setTimeout(() => {
  console.log("Ejecutado después de 500ms mínimos");
}, 500);

// Si se necesita cancelar antes de que corra:
clearTimeout(id);
```

## Firma completa

```js
setTimeout(fn, delay, arg1, arg2, ...)
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `fn` | Function | Callback a ejecutar |
| `delay` | number | Tiempo mínimo de espera en ms (defecto: 0) |
| `...args` | any | Argumentos que se pasan directamente a `fn` |

Los `...args` extra son útiles para evitar crear una closure innecesaria cuando solo se necesita pasar datos estáticos:

```js
function saludar(nombre, saludo) {
  console.log(`${saludo}, ${nombre}!`);
}

setTimeout(saludar, 1000, "Mikael", "Hola"); // → "Hola, Mikael!" tras ~1s
```

## Cómo funciona por dentro

`setTimeout` no pertenece al motor JavaScript sino al entorno (navegador / Node.js). Al llamarlo:

1. El entorno registra el timer en su propio scheduler.
2. El hilo JS continúa ejecutando el código síncrono.
3. Cuando expira el `delay`, el entorno encola `fn` en la **macrotask queue** (Task Queue).
4. El Event Loop procesa `fn` solo cuando el Call Stack está vacío y **todas las microtasks** (Promises, `queueMicrotask`) de la iteración actual se han vaciado.

Por eso `delay` es un mínimo, no un exacto. En la práctica el retraso real puede ser varios milisegundos mayor si el Call Stack tiene trabajo síncrono pendiente.

### setTimeout(fn, 0)

Con `delay = 0` el callback se encola en la siguiente iteración del Event Loop — útil para diferir código que debe correr después de que el stack actual se vacíe y las microtasks se procesen:

```js
console.log("1");
setTimeout(() => console.log("3 — macrotask"), 0);
Promise.resolve().then(() => console.log("2 — microtask"));
// Salida: 1 → 2 → 3
```

### Throttling en segundo plano

Los navegadores modernos limitan `setTimeout` a una resolución mínima de **1000ms** cuando la pestaña está en segundo plano (oculta o minimizada) para ahorrar batería y CPU. `requestAnimationFrame` directamente se pausa.

## Receta: Debounce de campo de búsqueda

Debounce cancela el timer anterior en cada nueva llamada y solo ejecuta la función cuando el usuario deja de escribir durante `delay` ms:

```js
function debounce(fn, delay) {
  let timerId;
  return function (...args) {
    clearTimeout(timerId);
    timerId = setTimeout(() => fn.apply(this, args), delay);
  };
}

const buscar = debounce((query) => {
  fetch(`/api/search?q=${query}`)
    .then(r => r.json())
    .then(console.log);
}, 300);

document.querySelector("#search").addEventListener("input", e => buscar(e.target.value));
```

## Receta: Retraso de tooltip

Mostrar un tooltip solo si el usuario mantiene el cursor durante 400ms, ocultarlo inmediatamente al salir:

```js
let tooltipTimer;

trigger.addEventListener("mouseenter", () => {
  tooltipTimer = setTimeout(() => tooltip.classList.add("visible"), 400);
});

trigger.addEventListener("mouseleave", () => {
  clearTimeout(tooltipTimer);
  tooltip.classList.remove("visible");
});
```

## Receta: Timeout de Promise con Promise.race

Rechazar una operación asíncrona si tarda más de N milisegundos:

```js
function withTimeout(promise, ms) {
  const timeout = new Promise((_, reject) =>
    setTimeout(() => reject(new Error(`Timeout tras ${ms}ms`)), ms)
  );
  return Promise.race([promise, timeout]);
}

withTimeout(fetch("/api/data"), 5000)
  .then(r => r.json())
  .then(console.log)
  .catch(err => console.error(err.message));
```

> [!tip] Limpia siempre los timers
> En componentes de UI que se montan y desmontan, guarda el `timeoutId` y llama `clearTimeout` en la función de cleanup (efecto de React, `disconnectedCallback` en Custom Elements). De lo contrario los callbacks se ejecutan sobre un componente ya destruido.

> [!warning] No usar setTimeout para medir rendimiento
> `setTimeout(fn, 0)` no garantiza cero latencia — en navegadores modernos el mínimo real es ~4ms (o 1ms en Node). Para medir tiempos precisos usar `performance.now()`.

## Notas relacionadas

- [[index|Temporizadores — resumen de las APIs]]
- [[02 setInterval|setInterval — ejecución periódica]]
- [[03 requestAnimationFrame|requestAnimationFrame — sincronizado con el render]]
