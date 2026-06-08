---
title: setInterval — ejecución periódica en JavaScript
aliases:
  - setInterval
  - clearInterval
tags:
  - javascript
  - api/metodo
  - asincrono
draft: false
---

# setInterval

> [!definicion]
> `setInterval(fn, delay, ...args)` encola `fn` como macrotask cada `delay` milisegundos de forma indefinida. Devuelve un `intervalId` (número entero) que se usa para detener el interval con `clearInterval(intervalId)`.

```js
const id = setInterval(() => {
  console.log("tick:", new Date().toLocaleTimeString());
}, 1000);

// Detener después de 5 segundos:
setTimeout(() => clearInterval(id), 5000);
```

## Firma completa

```js
setInterval(fn, delay, arg1, arg2, ...)
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `fn` | Function | Callback a ejecutar en cada intervalo |
| `delay` | number | Tiempo entre ejecuciones en ms |
| `...args` | any | Argumentos pasados a `fn` en cada llamada |

## El problema del drift

`setInterval` mide el tiempo desde que **encola** el callback, no desde que termina de ejecutarse. Si `fn` tarda más que `delay`, el siguiente callback se encola inmediatamente (el intervalo no espera a que el anterior termine):

```
delay = 100ms, fn tarda 120ms:

t=0    encola fn[1]
t=100  encola fn[2] (fn[1] aún no terminó)
t=120  fn[1] termina
t=120  fn[2] empieza inmediatamente (ya estaba encolado)
t=200  encola fn[3]
...
```

El resultado es que el intervalo "se acumula" — la separación real entre ejecuciones se reduce progresivamente si el callback es costoso.

## Cómo funciona por dentro

Al igual que `setTimeout`, el timer vive en el scheduler del entorno. En cada tick del intervalo el entorno añade una nueva macrotask a la Task Queue. Si el Event Loop está ocupado procesando otras tareas, el callback se ejecutará tarde, pero el siguiente intervalo ya habrá sido programado desde el momento del tick anterior (no desde el fin de la ejecución).

## Alternativa sin drift: setTimeout recursivo

Para intervalos donde el tiempo entre el fin de una ejecución y el inicio de la siguiente debe ser `delay` ms exactos, usar `setTimeout` recursivo:

```js
function setAdaptiveInterval(fn, delay) {
  let active = true;

  async function tick() {
    const start = performance.now();
    await fn();
    const elapsed = performance.now() - start;
    const next = Math.max(0, delay - elapsed); // no esperar más que necesario
    if (active) setTimeout(tick, next);
  }

  setTimeout(tick, delay);
  return () => { active = false; };
}

const stop = setAdaptiveInterval(async () => {
  const data = await fetch("/api/metrics").then(r => r.json());
  actualizarUI(data);
}, 5000);

// Limpiar cuando ya no se necesite:
// stop();
```

## Receta: Reloj de cuenta atrás

```js
function countdown(seconds, onTick, onEnd) {
  let remaining = seconds;
  onTick(remaining);

  const id = setInterval(() => {
    remaining--;
    onTick(remaining);
    if (remaining <= 0) {
      clearInterval(id);
      onEnd();
    }
  }, 1000);

  return id; // para poder cancelar externamente
}

countdown(
  10,
  (s) => (document.querySelector("#timer").textContent = s),
  () => console.log("¡Tiempo!")
);
```

## Receta: Polling de API cada 5 segundos

Patrón común en dashboards: pedir datos al servidor periódicamente y limpiar el interval cuando el componente se destruye:

```js
class DataPoller {
  #intervalId = null;

  start(url, onData, onError, interval = 5000) {
    const poll = () =>
      fetch(url)
        .then(r => r.json())
        .then(onData)
        .catch(onError);

    poll(); // primera llamada inmediata
    this.#intervalId = setInterval(poll, interval);
  }

  stop() {
    clearInterval(this.#intervalId);
    this.#intervalId = null;
  }
}

const poller = new DataPoller();
poller.start("/api/metrics", actualizarGrafica, console.error);

// Al desmontar el componente:
// poller.stop();
```

> [!tip] setInterval vs requestAnimationFrame para animaciones
> `setInterval(fn, 0)` o con delay pequeño no produce un bucle fiel de 60fps — el scheduling de macrotasks es impreciso a esa resolución. Para animaciones visuales, siempre usar `requestAnimationFrame`: está sincronizado con el ciclo de render del navegador y se pausa solo cuando la pestaña no es visible.

> [!warning] Limpia siempre el interval
> Un `setInterval` sin `clearInterval` es una fuga de recursos: sigue ejecutando callbacks aunque el componente que lo creó ya no exista, lo que puede provocar errores al intentar actualizar un DOM desmontado o referencias a closures que impiden la recolección de basura.

## Notas relacionadas

- [[index|Temporizadores — resumen de las APIs]]
- [[01 setTimeout|setTimeout — retardo único y debounce]]
- [[03 requestAnimationFrame|requestAnimationFrame — sincronizado con el render]]
