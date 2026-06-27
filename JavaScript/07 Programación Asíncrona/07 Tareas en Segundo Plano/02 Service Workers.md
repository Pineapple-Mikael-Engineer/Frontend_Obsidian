---
title: Service Workers — proxy de red y núcleo de las PWA
aliases:
  - Service Worker
  - SW
  - service worker
tags:
  - javascript
  - api/web
  - asincrono
objeto: Web API
tipo: concepto
retorna: ServiceWorkerRegistration
muta: false
asincrono: true
draft: false
order: 2
---

# Service Workers

> [!definicion]
> Un **Service Worker** es un worker especial que actúa como proxy programable entre la aplicación web y la red. Se registra por origen, persiste entre navegaciones y puede ejecutarse aunque la página esté cerrada. Es el mecanismo que habilita las Progressive Web Apps (PWA): caché offline, push notifications, background sync y control fino sobre cada petición de red.

```js
// hilo principal — registrar el Service Worker
if ("serviceWorker" in navigator) {
  navigator.serviceWorker.register("/sw.js")
    .then(reg => console.log("SW registrado:", reg.scope))
    .catch(err => console.error("Registro fallido:", err));
}
```

Solo funciona en **HTTPS** (o `localhost`). El scope por defecto es el directorio donde reside `sw.js`; se puede ampliar con `{ scope: "/" }`.

## Ciclo de vida

```
register()
    ↓
install       ← precachear recursos estáticos (e.waitUntil)
    ↓
waiting       ← nueva versión espera a que no queden clientes con la anterior
    ↓
activate      ← limpiar cachés viejas (e.waitUntil)
    ↓
fetch / push / sync   ← interceptar peticiones y eventos
```

Cada nueva versión de `sw.js` pasa por `install` → `waiting` → `activate`. Para forzar activación inmediata (sin esperar cierre de pestañas): `self.skipWaiting()` en `install` + `clients.claim()` en `activate`.

## Evento install — precacheo

```js
// sw.js
const CACHE_NAME = "v1";
const ASSETS = ["/", "/index.html", "/app.js", "/styles.css", "/offline.html"];

self.addEventListener("install", (e) => {
  e.waitUntil(
    caches.open(CACHE_NAME).then(cache => cache.addAll(ASSETS))
  );
  self.skipWaiting(); // activar en cuanto termine install
});
```

`e.waitUntil(promesa)` mantiene al SW en la fase actual hasta que la promesa se resuelva. Si rechaza, el SW se descarta.

## Evento fetch — interceptar peticiones

```js
// sw.js
self.addEventListener("fetch", (e) => {
  e.respondWith(
    caches.match(e.request).then(cached => cached ?? fetch(e.request))
  );
});
```

`e.respondWith(promesa<Response>)` sustituye la respuesta de red. Si no se llama, la petición continúa normalmente.

## Estrategias de caché

| Estrategia | Comportamiento | Cuándo usarla |
|---|---|---|
| Cache First | Sirve caché; si falla, red | Assets estáticos con versión (JS, CSS, fuentes) |
| Network First | Intenta red; si falla, caché | Datos que deben estar frescos pero toleran stale |
| Stale While Revalidate | Sirve caché inmediatamente y actualiza en segundo plano | Contenido que cambia poco pero debe responder rápido |
| Cache Only | Solo caché, nunca red | Recursos precacheados con garantía |
| Network Only | Solo red, nunca caché | Peticiones críticas (pagos, analítica) |

### Stale While Revalidate — implementación

```js
self.addEventListener("fetch", (e) => {
  e.respondWith(
    caches.open(CACHE_NAME).then(async (cache) => {
      const cached = await cache.match(e.request);
      const networkFetch = fetch(e.request).then(res => {
        cache.put(e.request, res.clone()); // actualizar caché en segundo plano
        return res;
      });
      return cached ?? networkFetch; // responder inmediatamente con caché si existe
    })
  );
});
```

### Offline fallback

```js
self.addEventListener("fetch", (e) => {
  if (e.request.mode === "navigate") {
    e.respondWith(
      fetch(e.request).catch(() => caches.match("/offline.html"))
    );
  }
});
```

## Evento activate — limpiar cachés viejas

```js
self.addEventListener("activate", (e) => {
  const CACHES_VALIDAS = [CACHE_NAME];
  e.waitUntil(
    caches.keys().then(nombres =>
      Promise.all(
        nombres
          .filter(nombre => !CACHES_VALIDAS.includes(nombre))
          .map(nombre => caches.delete(nombre))
      )
    ).then(() => self.clients.claim()) // tomar control inmediato de clientes activos
  );
});
```

## Push Notifications

El SW puede recibir mensajes push del servidor aunque la página esté cerrada, a través de la Push API.

```js
// sw.js
self.addEventListener("push", (e) => {
  const datos = e.data?.json() ?? { title: "Notificación", body: "" };
  e.waitUntil(
    self.registration.showNotification(datos.title, {
      body: datos.body,
      icon: "/icon-192.png",
      badge: "/badge.png",
    })
  );
});

self.addEventListener("notificationclick", (e) => {
  e.notification.close();
  e.waitUntil(clients.openWindow("/"));
});
```

El navegador mantiene al SW activo hasta que `waitUntil` resuelva. La suscripción push se gestiona desde el hilo principal con `registration.pushManager.subscribe()`.

## Background Sync

Permite reintentar operaciones fallidas (formularios, peticiones POST) cuando se restaura la conexión.

```js
// hilo principal — registrar sincronización
async function enviarConReintentos(datos) {
  const reg = await navigator.serviceWorker.ready;
  await guardarEnIndexedDB(datos);        // persistir localmente primero
  await reg.sync.register("sync-mensajes");
}

// sw.js
self.addEventListener("sync", (e) => {
  if (e.tag === "sync-mensajes") {
    e.waitUntil(
      leerDeIndexedDB().then(items =>
        Promise.all(items.map(item => fetch("/api/mensajes", { method: "POST", body: JSON.stringify(item) })))
      )
    );
  }
});
```

## Cómo funciona por dentro

El SW vive en un thread dedicado del navegador con su propio event loop. Al recibir un evento (`fetch`, `push`, `sync`), el navegador lo despierta si estaba dormido (el tiempo de arranque es mínimo porque el estado persiste). Tras procesar el evento, el navegador puede suspender el SW para liberar recursos. El SW no tiene acceso a ningún objeto de la página (`window`, DOM, objetos de JS del hilo principal); la comunicación bidireccional con los clientes se hace a través de `Client.postMessage()` / `navigator.serviceWorker.controller.postMessage()`.

La **Cache API** que usa el SW es distinta del caché HTTP del navegador: es programática, vive en el storage del origen y persiste indefinidamente hasta que el código la elimine o el navegador la limpie por presión de almacenamiento.

## Herramientas — Workbox

Google Workbox abstrae la implementación de estrategias de caché y el ciclo de vida del SW:

```js
// sw.js con Workbox
import { registerRoute } from "workbox-routing";
import { StaleWhileRevalidate, CacheFirst } from "workbox-strategies";
import { precacheAndRoute } from "workbox-precaching";

precacheAndRoute(self.__WB_MANIFEST); // inyectado por el build tool

registerRoute(
  ({ request }) => request.destination === "image",
  new CacheFirst({ cacheName: "imagenes", plugins: [/* ExpirationPlugin */] })
);

registerRoute(
  ({ request }) => request.destination === "document",
  new StaleWhileRevalidate({ cacheName: "paginas" })
);
```

> [!tip]
> Usar `workbox-window` en el hilo principal para detectar actualizaciones del SW y notificar al usuario con un banner "Nueva versión disponible — recargar". El flujo `skipWaiting` + `clients.claim()` + notificación controlada es el estándar de producción.

> [!warning]
> El SW intercepta **todas** las peticiones de su scope. Un bug en el evento `fetch` (como no llamar `e.respondWith` correctamente o devolver una Promise que rechaza) puede romper la red para toda la aplicación. Testear siempre con las DevTools → Application → Service Workers → "Bypass for network" durante el desarrollo. Versionar explícitamente los nombres de caché (`v1`, `v2`…) para que el activate limpie correctamente.

## Notas relacionadas

- [[01 Web Workers|Web Workers]] — workers para cómputo intensivo, sin ciclo de vida de red
- [[07 Tareas en Segundo Plano/index|Tareas en Segundo Plano]] — panorama de Workers en JS
- [[06 Async - Await/index|Async / Await]] — el modelo de `async/await` que usan los handlers del SW
