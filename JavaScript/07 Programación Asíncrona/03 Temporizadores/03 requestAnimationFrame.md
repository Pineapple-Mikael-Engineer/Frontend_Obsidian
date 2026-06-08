---
title: requestAnimationFrame — sincronizado con el ciclo de render
aliases:
  - requestAnimationFrame
  - rAF
  - cancelAnimationFrame
tags:
  - javascript
  - api/metodo
  - asincrono
draft: false
---

# requestAnimationFrame

> [!definicion]
> `requestAnimationFrame(fn)` encola `fn` para que se ejecute inmediatamente antes del próximo **repaint** del navegador. Devuelve un `requestId` (número entero) que puede cancelarse con `cancelAnimationFrame(requestId)`. El callback recibe un `DOMHighResTimeStamp` con el tiempo en ms desde la carga de la página.

```js
function animar(timestamp) {
  // timestamp: ms desde performance.timeOrigin
  actualizarEstado(timestamp);
  dibujarFrame();
  requestAnimationFrame(animar); // solicitar el siguiente frame
}

const id = requestAnimationFrame(animar);

// Para detener:
// cancelAnimationFrame(id);
```

## Firma

```js
const requestId = requestAnimationFrame(callback);

// callback recibe:
// timestamp: DOMHighResTimeStamp — ms desde la carga de la página, precisión sub-ms
```

## Cómo funciona por dentro

El navegador tiene un **ciclo de render** que ocurre ~60 veces por segundo en un monitor de 60 Hz (cada ~16.67ms). El ciclo incluye: procesar estilos → layout → paint → composite. `requestAnimationFrame` inserta el callback justo al inicio de ese ciclo, antes de que el navegador calcule el layout y pinte:

```
Event Loop → [scripts] → [rAF callbacks] → style → layout → paint → composite
```

Esto garantiza que cualquier modificación del DOM o CSS que hagas dentro del callback se procese en el mismo frame — sin layouts forzados (reflows) adicionales.

### Adaptación automática al frame rate

`rAF` no fija el intervalo en 16ms — se adapta al frame rate real del dispositivo:
- Monitor de 60 Hz → ~16.67ms por frame
- Monitor de 120 Hz → ~8.33ms por frame
- Monitor de 144 Hz → ~6.94ms por frame

Un `setInterval(fn, 16)` en un monitor de 120 Hz produciría el doble de frames de los que el monitor puede mostrar — frames desperdiciados. `rAF` siempre produce exactamente un callback por frame visible.

### Pausa automática en segundo plano

Cuando la pestaña no es visible (minimizada, en segundo plano, o cubierta), el navegador pausa el ciclo de render. Los callbacks de `requestAnimationFrame` **no se ejecutan** — se acumulan y se procesan cuando la pestaña vuelve a ser visible. Esto ahorra batería y CPU sin ningún código extra.

## Receta: Animación de contador con rAF

Animar el valor de un contador desde `from` hasta `to` durante `duration` ms con easing lineal:

```js
function animateCounter(from, to, duration, onUpdate) {
  const start = performance.now();

  function step(now) {
    const progress = Math.min((now - start) / duration, 1);
    onUpdate(Math.round(from + (to - from) * progress));
    if (progress < 1) requestAnimationFrame(step);
  }

  requestAnimationFrame(step);
}

animateCounter(0, 1000, 2000, (value) => {
  document.querySelector("#counter").textContent = value;
});
```

La clave es pasar `step` como referencia a `requestAnimationFrame` desde dentro de `step` — cada frame solicita el siguiente, y la animación para naturalmente cuando `progress` alcanza 1.

## Receta: Batching de actualizaciones del DOM en un único frame

Cuando múltiples eventos disparan actualizaciones del DOM en la misma iteración del Event Loop, agruparlas en un solo frame evita repaints innecesarios:

```js
let pendingUpdate = false;
let pendingData = null;

function scheduleUpdate(data) {
  pendingData = data;
  if (!pendingUpdate) {
    pendingUpdate = true;
    requestAnimationFrame(() => {
      renderizarLista(pendingData);
      pendingUpdate = false;
    });
  }
}

// Múltiples llamadas en el mismo tick:
scheduleUpdate({ items: [...] }); // solo programa un rAF
scheduleUpdate({ items: [...] }); // el flag pendingUpdate lo ignora
```

## Por qué rAF es superior a setInterval para animaciones

| Criterio | setInterval | requestAnimationFrame |
|---|---|---|
| Sincronización con el render | No (puede ejecutar entre frames) | Sí (siempre justo antes del repaint) |
| Adaptación al frame rate | No (intervalo fijo) | Sí (se adapta a 60/120/144 Hz) |
| Comportamiento en segundo plano | Sigue ejecutando (gasta CPU) | Se pausa solo |
| Frames perdidos | Posible (drift y scheduling impreciso) | No (un callback por frame visible) |
| Precisión del timestamp | ms desde epoch (Date.now) | DOMHighResTimeStamp sub-ms |

> [!tip] Combinar rAF con performance.now()
> El `timestamp` que recibe el callback es el mismo para todos los callbacks del mismo frame. Si necesitas calcular el delta entre frames para animaciones físicas (velocidad, aceleración), usa `const delta = timestamp - lastTimestamp` y actualiza `lastTimestamp = timestamp` al final del callback.

> [!warning] No mezclar lógica de negocio con rAF
> `requestAnimationFrame` debe usarse exclusivamente para tareas visuales. Colocar lógica pesada (cálculos, parseo de datos, peticiones de red) dentro de un callback de `rAF` bloquea el hilo justo antes del repaint, causando jank (frames perdidos). La lógica costosa va en Workers o en macrotasks separadas.

## Notas relacionadas

- [[index|Temporizadores — resumen de las APIs]]
- [[01 setTimeout|setTimeout — retardo único]]
- [[02 setInterval|setInterval — ejecución periódica]]
