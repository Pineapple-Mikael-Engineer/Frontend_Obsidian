---
title: Performance API — métricas de rendimiento de alta resolución
aliases:
  - Performance API
  - performance.now
  - PerformanceObserver
  - Web Vitals
tags:
  - javascript
  - api/objeto
  - browser
  - rendimiento
objeto: Web API
tipo: concepto
draft: false
order: 1
---

# Performance API

> [!definicion]
> La **Performance API** proporciona acceso a métricas de rendimiento del navegador con alta precisión temporal. El objeto global `performance` expone un reloj de sub-milisegundo (`performance.now()`), un sistema de marcas y medidas personalizadas (`mark`/`measure`), y una interfaz para consultar u observar en tiempo real las entradas de rendimiento del browser (`PerformanceEntry`, `PerformanceObserver`). Está definida por la especificación W3C High Resolution Time y el User Timing API.

```js
// Medir el tiempo de una operación
performance.mark("inicio-render");
renderizarComponente();
performance.mark("fin-render");

const medida = performance.measure("render", "inicio-render", "fin-render");
console.log(`Render: ${medida.duration.toFixed(2)} ms`);

performance.clearMarks();
performance.clearMeasures();
```

## `performance.now()`

Retorna un `DOMHighResTimeStamp` (número de punto flotante en milisegundos) relativo al inicio de la navegación de la página. La resolución es de sub-milisegundo (hasta microsegundos en algunos entornos), lo que lo hace significativamente más preciso que `Date.now()` (resolución de 1 ms, basado en epoch UNIX).

```js
const t0 = performance.now();
operacionCostosa();
const t1 = performance.now();
console.log(`Duración: ${(t1 - t0).toFixed(3)} ms`);
// → "Duración: 12.847 ms"
```

Por razones de seguridad (ataques Spectre), los browsers pueden reducir la resolución de `performance.now()` a 0.1 ms o 5 ms según el contexto de seguridad (cross-origin isolation).

## User Timing API — `mark` y `measure`

```js
performance.mark("nombre-mark");                          // registrar un timestamp con nombre
performance.measure("nombre-medida", "mark-inicio", "mark-fin"); // medir entre dos marks
performance.measure("desde-inicio", undefined, "mark-fin");       // desde el inicio de la navegación

performance.getEntriesByName("nombre-medida");   // → [PerformanceMeasure]
performance.getEntriesByType("mark");            // → [PerformanceMark, ...]
performance.getEntriesByType("measure");         // → [PerformanceMeasure, ...]

performance.clearMarks("nombre-mark");           // eliminar una mark concreta
performance.clearMarks();                        // eliminar todas las marks
performance.clearMeasures();                     // eliminar todas las measures
```

## Tipos de `PerformanceEntry`

| Tipo (`entryType`) | Qué mide |
|---|---|
| `"navigation"` | Tiempos completos de navegación: DNS, TCP, TTFB, DOMContentLoaded, `load` |
| `"resource"` | Tiempos de carga de cada recurso externo: imágenes, scripts, CSS, fuentes |
| `"paint"` | First Paint (FP) y First Contentful Paint (FCP) |
| `"largest-contentful-paint"` | LCP — Largest Contentful Paint (Core Web Vital) |
| `"layout-shift"` | CLS — Cumulative Layout Shift (Core Web Vital) |
| `"long-task"` | Tareas en el hilo principal que superan los 50 ms |
| `"longtask"` | Alias de `long-task` en algunos browsers |
| `"event"` | INP — Interaction to Next Paint (Core Web Vital, reemplaza FID) |
| `"user-timing"` | Marks y measures personalizadas |
| `"element"` | Tiempo de renderizado de elementos específicos |

## `PerformanceObserver` — observar en tiempo real

`PerformanceObserver` es la forma correcta de capturar entradas que llegan de forma asíncrona (LCP, Long Tasks, Layout Shifts). El método polling con `getEntriesByType()` solo captura entradas ya registradas al momento de la llamada; las entradas futuras se pierden.

```js
const observer = new PerformanceObserver(list => {
  list.getEntries().forEach(entry => {
    console.log(entry.entryType, entry.name, entry.startTime, entry.duration);
  });
});

observer.observe({ entryTypes: ["largest-contentful-paint", "long-task", "layout-shift"] });

// Obtener las entradas ya registradas al iniciar la observación
observer.observe({ type: "largest-contentful-paint", buffered: true });

observer.disconnect(); // dejar de observar
```

La opción `buffered: true` en `observe()` entrega también las entradas que se produjeron antes de que el observer fuera creado (disponibles en el buffer del browser). Esencial para capturar LCP en páginas que ya cargaron.

## Core Web Vitals medibles con Performance API

| Métrica | Tipo de entrada | Qué mide | Umbral bueno |
|---|---|---|---|
| LCP (Largest Contentful Paint) | `largest-contentful-paint` | Tiempo hasta que el elemento de contenido más grande es visible | < 2.5 s |
| INP (Interaction to Next Paint) | `event` | Latencia de respuesta a interacciones del usuario | < 200 ms |
| CLS (Cumulative Layout Shift) | `layout-shift` | Estabilidad visual; acumula desplazamientos de layout inesperados | < 0.1 |

FID (First Input Delay) está deprecado y reemplazado por INP desde 2024.

## `PerformanceNavigationTiming`

Hereda de `PerformanceResourceTiming` y expone los tiempos completos de la navegación. Se obtiene con `performance.getEntriesByType("navigation")[0]`:

```js
const nav = performance.getEntriesByType("navigation")[0];
console.log("TTFB:", nav.responseStart - nav.requestStart, "ms");
console.log("DOM interactive:", nav.domInteractive, "ms");
console.log("DOM complete:", nav.domComplete, "ms");
console.log("Load event:", nav.loadEventEnd - nav.loadEventStart, "ms");
```

La API más antigua `performance.timing` (PerformanceTiming) está deprecada; `PerformanceNavigationTiming` la reemplaza con mayor precisión y el mismo modelo de entradas.

## Receta: instrumentar una función

```js
function medirFuncion(nombre, fn) {
  return function(...args) {
    const start = `${nombre}-start`;
    const end   = `${nombre}-end`;
    performance.mark(start);
    const resultado = fn.apply(this, args);
    performance.mark(end);
    const entry = performance.measure(nombre, start, end);
    console.log(`[perf] ${nombre}: ${entry.duration.toFixed(2)} ms`);
    performance.clearMarks(start);
    performance.clearMarks(end);
    return resultado;
  };
}

const procesarDatos = medirFuncion("procesarDatos", datos => {
  // lógica costosa
  return datos.map(transformar);
});
```

## Cómo funciona por dentro

`performance.now()` usa el reloj monotónico del sistema operativo (no ajustable por el usuario ni afectado por cambios de hora del sistema), relativo al `timeOrigin` de la página (`performance.timeOrigin` en ms desde UNIX epoch). Las `PerformanceEntry` se almacenan en un buffer interno del browser; los observers reciben notificaciones cuando se añaden nuevas entradas a ese buffer, de forma asíncrona y sin bloquear el hilo principal.

> [!tip]
> Para medir el rendimiento de código asíncrono, usar `performance.mark` antes y después de la operación completa (incluyendo las promesas resueltas): `mark("antes")` → `await operacion()` → `mark("después")` → `measure(...)`. Los timestamps se asignan en el momento exacto de la llamada a `mark`, independientemente de cuando se procese la cola de microtasks.

> [!warning]
> `performance.getEntriesByType("long-task")` requiere que el browser soporte la Long Tasks API (todos los navegadores Chromium; Firefox tiene soporte limitado; Safari, pendiente). Para detectar soporte: `PerformanceObserver.supportedEntryTypes.includes("longtask")`. Para medir el tiempo de respuesta en todos los browsers, `performance.now()` con `mark`/`measure` es universalmente soportado.

## Notas relacionadas

- [[06 Observers/index|Observers]] — `PerformanceObserver` sigue el mismo patrón que `IntersectionObserver` y `MutationObserver`.
- [[../07 Programación Asíncrona/index|Programación Asíncrona]] — entender el event loop es necesario para interpretar correctamente los timestamps de rendimiento.
- [[../11 Manejo de Errores y Depuración/index|Manejo de Errores y Depuración]] — la Performance API complementa las herramientas de DevTools para profiling en producción.
