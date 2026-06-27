---
title: Temporizadores — APIs de ejecución diferida en JavaScript
aliases:
  - temporizadores js
  - timers javascript
tags:
  - javascript
  - api/concepto
  - asincrono
draft: false
order: 3
---

# Temporizadores

> [!definicion]
> Los **temporizadores** son APIs del entorno (navegador / Node.js) que permiten ejecutar código de forma diferida o repetida sin bloquear el hilo principal. No forman parte de la especificación ECMAScript sino de las Web APIs y el entorno Node.js.

Los tres mecanismos cubren casos distintos: retardo único (`setTimeout`), ejecución periódica (`setInterval`) y sincronización con el ciclo de render del navegador (`requestAnimationFrame`). Los tres son asíncronos — el callback se encola en alguna cola del Event Loop, nunca interrumpe la ejecución síncrona en curso.

## Comparativa de APIs

| API | Propósito | Resolución de tiempo | Cancela con |
|---|---|---|---|
| `setTimeout(fn, delay)` | Ejecutar `fn` una vez tras `delay` ms | ~1ms (mínimo, no garantizado) | `clearTimeout(id)` |
| `setInterval(fn, delay)` | Ejecutar `fn` cada `delay` ms | ~1ms (mínimo, no garantizado) | `clearInterval(id)` |
| `requestAnimationFrame(fn)` | Ejecutar `fn` antes del próximo repaint | ~16ms a 60 fps (variable según dispositivo) | `cancelAnimationFrame(id)` |

> [!tip] Qué API usar
> Para lógica de negocio con retardo: `setTimeout`. Para polling o actualizaciones periódicas de datos: `setInterval` con `setTimeout` recursivo si el intervalo debe ser exacto. Para cualquier animación visual o actualización del DOM que deba verse fluida: `requestAnimationFrame`.

> [!warning] `delay` no es exacto
> El `delay` en `setTimeout` y `setInterval` es un mínimo: el Event Loop puede retrasarlo si hay macrotasks o microtasks pendientes, y los navegadores modernos aplican throttling de 1000ms en pestañas en segundo plano.

## Notas relacionadas

- [[01 setTimeout|setTimeout — retardo único]]
- [[02 setInterval|setInterval — ejecución periódica]]
- [[03 requestAnimationFrame|requestAnimationFrame — sincronizado con el render]]
