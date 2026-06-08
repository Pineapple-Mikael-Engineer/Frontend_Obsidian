---
title: requestAnimationFrame
aliases:
  - requestAnimationFrame
  - rAF
  - cancelAnimationFrame
tags:
  - javascript
  - api/metodo
  - dom
draft: false
---

# `requestAnimationFrame`

> [!definicion]
> `requestAnimationFrame(callback)` programa la ejecución del `callback` justo antes del próximo **repaint** del navegador. El callback recibe un `DOMHighResTimeStamp` — el tiempo transcurrido desde el origen de la página — y devuelve un ID entero que permite cancelar la ejecución con `cancelAnimationFrame(id)`.

```js
const id = requestAnimationFrame(callback);
cancelAnimationFrame(id); // cancelar antes de que se ejecute
```

El callback se ejecuta a ~60fps (16.7 ms entre frames) en monitores de 60 Hz, pero la frecuencia se adapta automáticamente a la tasa de refresco del monitor (90, 120, 144 Hz…).

## Loop de animación

El patrón estándar es que el callback se reprograme a sí mismo al final de cada frame:

```js
let posicion = 0;

function animar(timestamp) {
  // timestamp: ms desde el inicio de la navegación (DOMHighResTimeStamp)
  posicion = Math.sin(timestamp / 1000) * 100; // oscilación
  el.style.transform = `translateX(${posicion}px)`;

  requestAnimationFrame(animar); // reprogramar para el siguiente frame
}

const idInicial = requestAnimationFrame(animar); // arrancar
```

Para detener el loop:

```js
let animId;

function arrancar() { animId = requestAnimationFrame(animar); }
function detener()  { cancelAnimationFrame(animId); }
```

## Por qué rAF es mejor que `setTimeout` para animaciones

`setTimeout(fn, 16)` no está sincronizado con el ciclo de renderizado del navegador: el callback puede ejecutarse en cualquier punto del frame, lo que produce actualizaciones que llegan antes o después del repaint y causan saltos visuales (tearing). `requestAnimationFrame` garantiza que el callback se ejecuta en la ventana de tiempo correcta — el motor puede agrupar la ejecución del JS con el cálculo de estilos y el layout del mismo frame.

| | `setTimeout(fn, 16)` | `requestAnimationFrame` |
|---|---|---|
| Sincronización con repaint | No | Sí |
| Tasa adaptativa al monitor | No | Sí |
| Pausa en tab en background | No | Sí (ahorra CPU) |
| Timestamp preciso | No | Sí (`DOMHighResTimeStamp`) |

## Animación basada en tiempo (no en frames)

Para que la animación tenga la misma velocidad en monitores de 60 y 120 Hz, el progreso debe calcularse en función del timestamp, no del número de frames:

```js
const DURACION = 1000; // 1 segundo
let inicio = null;

function animar(timestamp) {
  if (!inicio) inicio = timestamp;
  const progreso = Math.min((timestamp - inicio) / DURACION, 1); // 0..1

  el.style.opacity = progreso.toString();

  if (progreso < 1) requestAnimationFrame(animar);
}

requestAnimationFrame(animar);
```

## Receta: leer dimensiones sin layout thrashing

rAF también sirve para diferir lecturas de layout al momento en que el motor ya ha calculado el layout del frame anterior, evitando reflows síncronos:

```js
// Leer después de que el navegador haya actualizado el layout
requestAnimationFrame(() => {
  const altura = el.offsetHeight; // sin forzar reflow adicional
  // usar altura...
});
```

## Tab en background

El navegador pausa los callbacks de rAF cuando la pestaña está oculta (`document.hidden === true`). El loop solo continúa cuando la pestaña vuelve a estar visible. Esto ahorra CPU y batería automáticamente, a diferencia de `setTimeout` / `setInterval` que siguen disparándose.

```js
// El loop se pausa automáticamente al cambiar de pestaña
function loop(ts) {
  actualizarEstado(ts);
  renderizar();
  requestAnimationFrame(loop);
}
requestAnimationFrame(loop);
```

## Cómo funciona por dentro

El motor mantiene una cola de callbacks de rAF. Antes de cada repaint, procesa todos los callbacks encolados en ese momento (los que se encolen durante la ejecución de estos callbacks van a la siguiente cola). El timestamp es el mismo para todos los callbacks del mismo frame, lo que permite sincronizar múltiples animaciones sin deriva.

> [!tip]
> Para animaciones simples de CSS (transiciones de `transform`, `opacity`, cambios de color), las animaciones CSS o la Web Animations API son preferibles a rAF: el navegador puede ejecutarlas en el hilo del compositor sin pasar por JS. Reservar rAF para animaciones que requieren lógica JS en cada frame.

> [!warning]
> No realizar operaciones DOM costosas ni lecturas de layout dentro del callback de rAF si producen reflows — se ejecuta justo antes del repaint, y un reflow síncrono en ese punto deshace la ventaja de la sincronización. Si se necesitan lecturas de layout, hacerlas al inicio del callback (antes de cualquier escritura).

## Notas relacionadas

- [[03 Reflows y Repaints]] — qué sucede en cada etapa del pipeline de renderizado
- [[01 DocumentFragment]] — reducir reflows en inserciones de nodos
- [[09 Actualizaciones Eficientes/index|Actualizaciones Eficientes]]
