---
title: Throttle — Limitar la cadencia de ejecución
aliases:
  - throttle
  - throttling
tags:
  - javascript
  - api/concepto
  - patrones
draft: false
---

# Throttle

> [!definicion]
> **Throttle** envuelve una función `fn` y garantiza que no se ejecute más de una vez cada `limit` ms, sin importar cuántas veces se invoque durante ese intervalo. A diferencia del debounce, no espera silencio: ejecuta periódicamente durante el burst.

```js
function throttle(fn, limit) {
  let inThrottle = false;
  return function(...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;
      setTimeout(() => { inThrottle = false; }, limit);
    }
  };
}

// Uso — handler de scroll que calcula la posición máximo 1 vez cada 100 ms
window.addEventListener('scroll', throttle(() => {
  const y = window.scrollY;
  header.classList.toggle('compacto', y > 100);
}, 100));
```

## Variante con timestamps (más precisa)

La implementación con flag de booleano introduce drift acumulativo porque reinicia el timer desde el momento de ejecución. La variante con `Date.now()` mide el tiempo real transcurrido:

```js
function throttle(fn, limit) {
  let lastTime = 0;
  return function(...args) {
    const now = Date.now();
    if (now - lastTime >= limit) {
      lastTime = now;
      fn.apply(this, args);
    }
  };
}
```

Esta versión no usa `setTimeout` — no hay timer pendiente al desmontar. Es la opción preferida para scroll y mousemove donde no importa la llamada del trailing edge.

## Leading y trailing

Al igual que [[01 Debounce|debounce]], las implementaciones completas aceptan opciones `leading` y `trailing`:

| Configuración | Cuándo ejecuta |
|---|---|
| `leading: true, trailing: false` | Solo al inicio de cada ventana de tiempo |
| `leading: false, trailing: true` | Solo al final de cada ventana |
| `leading: true, trailing: true` | Al inicio y al final (default de Lodash) |

## `requestAnimationFrame` como alternativa

Para eventos visuales, `requestAnimationFrame` (rAF) es más adecuado que throttle con timer: sincroniza la ejecución con el ciclo de render del navegador (~16 ms, 60 fps) y elimina el trabajo en frames que el navegador descartará.

```js
function throttleRaf(fn) {
  let rafId = null;
  return function(...args) {
    if (rafId) return; // ya hay un frame pendiente
    rafId = requestAnimationFrame(() => {
      fn.apply(this, args);
      rafId = null;
    });
  };
}

window.addEventListener('mousemove', throttleRaf((e) => {
  cursor.style.transform = `translate(${e.clientX}px, ${e.clientY}px)`;
}));
```

## Cómo funciona por dentro

En la variante con flag: la primera llamada dentro de una ventana de `limit` ms ejecuta `fn` y activa `inThrottle`. Llamadas subsiguientes durante ese intervalo son ignoradas. Al expirar el `setTimeout`, `inThrottle` vuelve a `false` y la próxima llamada puede ejecutar de nuevo. La variante con timestamps calcula `now - lastTime` en cada invocación — si el delta supera `limit`, ejecuta y actualiza `lastTime`.

## Casos de uso

**Scroll — mostrar/ocultar header:** throttle a 100–200 ms. No es necesario procesar cada píxel de desplazamiento.

**Resize — recalcular layout:** throttle a 200 ms. Los cambios de tamaño continuos durante el arrastre no requieren recalcular en cada evento.

**Mousemove — efectos visuales:** rAF; sincronizado con el render, no genera frames intermedios.

**Analytics — cursor heatmap:** throttle a 1000 ms para enviar posición del cursor máximo 1 vez/segundo al servidor.

```js
// Analytics: posición del cursor, máximo 1 req/s
const reportarPosicion = throttle((e) => {
  navigator.sendBeacon('/analytics/cursor', JSON.stringify({
    x: e.clientX,
    y: e.clientY,
    t: Date.now()
  }));
}, 1000);

document.addEventListener('mousemove', reportarPosicion);
```

## Diferencia clave con debounce

[[01 Debounce|Debounce]] espera silencio: si el usuario escribe sin parar durante 5 segundos, la función no se ejecuta hasta el segundo 5. Throttle ejecuta cada N ms durante ese mismo período — el usuario vería resultados actualizados periódicamente aunque no haya pausado.

| Pregunta | Debounce | Throttle |
|---|---|---|
| ¿Se ejecuta durante el burst? | No (solo al final) | Sí, periódicamente |
| ¿Garantiza ejecución si el burst no cesa? | No (sin maxWait) | Sí |
| Caso canónico | Búsqueda al terminar de escribir | Actualización de scroll/resize continua |

> [!tip]
> Para scroll, preferir la variante con timestamps sobre la de flag booleano: evita el drift y no deja timers colgados si el componente se desmonta.

> [!warning]
> Con `leading: false, trailing: true` (el throttle "tarda en arrancar"), la primera llamada no ejecuta inmediatamente — el usuario puede percibir un retardo visible en efectos visuales. Para UI reactiva, usar `leading: true`.

## Notas relacionadas

- [[01 Debounce|Debounce]]
- [[index|Debouncing y Throttling — Índice]]
