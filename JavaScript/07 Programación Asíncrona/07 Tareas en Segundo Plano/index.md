---
title: Tareas en Segundo Plano — Workers en JavaScript
aliases:
  - tareas segundo plano
  - background tasks js
  - workers javascript
tags:
  - javascript
  - api/concepto
  - asincrono
draft: false
order: 7
---

# Tareas en Segundo Plano

> [!definicion]
> JavaScript es **single-threaded**: el hilo principal ejecuta JS, gestiona el Event Loop y coordina el rendering. Las tareas en segundo plano son mecanismos que el navegador expone para delegar trabajo fuera de ese hilo: los **Web Workers** para cómputo intensivo sin acceso al DOM, y los **Service Workers** para interceptar la red y habilitar experiencias offline. Ambos se comunican con el hilo principal exclusivamente mediante paso de mensajes.

El hilo principal puede manejar cualquier operación, pero solo tolera ~50 ms de trabajo continuo sin bloquear la UI. Todo lo que supere ese umbral es candidato a un Worker.

## Mecanismos disponibles

| Mecanismo | Propósito principal | Acceso al DOM | Persiste sin pestaña | Intercepta red |
|---|---|---|---|---|
| Web Worker (dedicado) | Cómputo intensivo en background | No | No | No |
| SharedWorker | Estado compartido entre pestañas | No | No (mientras alguna pestaña esté abierta) | No |
| Service Worker | Proxy de red, caché, PWA | No | Sí | Sí |

La comunicación en todos los casos sigue el mismo patrón: `postMessage` (enviar) + `onmessage` (recibir), con datos transferidos por **structured clone** o **transferencia** de `ArrayBuffer`.

## Cuándo usar cada uno

```
¿La tarea es cómputo puro (hash, análisis, simulación)?
  → Web Worker

¿Necesita compartir estado entre pestañas del mismo origen?
  → SharedWorker

¿Necesita funcionar offline, interceptar peticiones, recibir push?
  → Service Worker
```

## Notas de esta sección

- [[01 Web Workers|Web Workers]] — hilo separado para cómputo intensivo; `postMessage`, structured clone, transferibles, `SharedArrayBuffer` + `Atomics`, `terminate()`.
- [[02 Service Workers|Service Workers]] — proxy de red; ciclo de vida install/activate/fetch, estrategias de caché, push notifications, background sync, Workbox.
