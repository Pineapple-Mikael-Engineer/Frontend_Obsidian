---
title: Debouncing y Throttling — Índice
aliases:
  - debounce throttle
  - control de frecuencia de eventos
tags:
  - javascript
  - api/concepto
  - patrones
draft: false
---

# Debouncing y Throttling

> [!definicion]
> **Debouncing** y **throttling** son dos técnicas para controlar la frecuencia de ejecución de una función cuando se dispara a través de eventos de alta frecuencia (scroll, resize, input, mousemove). Sin control, un handler de `scroll` puede ejecutarse decenas de veces por segundo; estas técnicas reducen la carga computacional y de red sin perder la reactividad de la interfaz.

El problema que resuelven:

```js
// Sin control — puede ejecutarse 50+ veces/segundo en scroll
window.addEventListener('scroll', () => {
  calcularPosicion(); // pesado
});

// Con throttle — máximo 1 vez cada 200 ms
window.addEventListener('scroll', throttle(calcularPosicion, 200));
```

## Diferencia conceptual

| Técnica | Cuándo ejecuta | Comportamiento durante el burst | Garantía |
|---|---|---|---|
| Debounce | Cuando cesa la actividad (fin del burst) | Reinicia el timer en cada llamada | Se ejecuta al menos una vez al parar |
| Throttle | Periódicamente durante el burst | Ignora las llamadas intermedias | Se ejecuta cada N ms mientras haya actividad |
| rAF | En cada frame del navegador (~16 ms) | Sincronizado con el render | Máximo 60 fps, no más |

**Debounce** es adecuado cuando solo importa el estado final (el texto que el usuario terminó de escribir). **Throttle** es adecuado cuando importa el progreso continuo pero a una cadencia razonable (posición del scroll mientras se desplaza).

## Contenido de la sección

| # | Nombre | Cubre |
|---|---|---|
| 01 | Debounce | Implementación, leading/trailing/maxWait, cancelación, recetas |
| 02 | Throttle | Implementación con flag e timestamps, rAF como alternativa, casos de uso |

## Notas relacionadas

- [[01 Debounce|Debounce]]
- [[02 Throttle|Throttle]]
