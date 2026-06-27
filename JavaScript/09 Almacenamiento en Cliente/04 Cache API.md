---
title: Cache API — almacenamiento de pares Request/Response
aliases:
  - Cache API
  - CacheStorage
  - caches
tags:
  - javascript
  - api/web
  - almacenamiento
  - pwa
objeto: caches
tipo: concepto
asincrono: true
draft: false
order: 1
---

# Cache API

> [!definicion]
> La **Cache API** es una interfaz de almacenamiento persistente de pares `Request`/`Response` en el navegador. Está basada completamente en Promises y diseñada para usarse desde **Service Workers** como soporte de estrategias de caché para PWAs, aunque también es accesible desde el hilo principal. A diferencia de `localStorage`, almacena respuestas HTTP completas (incluyendo headers y cuerpos binarios) y no está limitada a strings. El espacio disponible está sujeto a la cuota del origen gestionada por el navegador.

```js
// Desde el hilo principal o un Service Worker
const cache = await caches.open("assets-v1");

await cache.add("/styles/main.css");          // fetch + guarda el par
await cache.addAll(["/app.js", "/logo.png"]); // batch fetch + guarda

const response = await cache.match("/styles/main.css");
// → Response con headers, status, body completo
```

## API de CacheStorage (`caches`)

`caches` es el objeto global de tipo `CacheStorage` disponible en Service Workers y en el hilo principal (en contextos seguros, HTTPS o localhost).

| Método | Retorna | Descripción |
|---|---|---|
| `caches.open(nombre)` | `Promise<Cache>` | Abre la caché con ese nombre; la crea si no existe |
| `caches.has(nombre)` | `Promise<boolean>` | Comprueba si existe una caché con ese nombre |
| `caches.keys()` | `Promise<string[]>` | Lista los nombres de todas las cachés del origen |
| `caches.delete(nombre)` | `Promise<boolean>` | Elimina toda una caché nombrada |
| `caches.match(request)` | `Promise<Response \| undefined>` | Busca en todas las cachés del origen |

## API de Cache (instancia)

| Método | Retorna | Descripción |
|---|---|---|
| `cache.put(request, response)` | `Promise<void>` | Guarda el par manualmente (sin fetch) |
| `cache.add(url)` | `Promise<void>` | Hace `fetch(url)` y guarda el par resultante |
| `cache.addAll(urls)` | `Promise<void>` | Batch de `add()`; falla todo si uno falla |
| `cache.match(request, options?)` | `Promise<Response \| undefined>` | Busca la respuesta cacheada para esa request |
| `cache.matchAll(request?, options?)` | `Promise<Response[]>` | Todas las respuestas que coinciden |
| `cache.delete(request, options?)` | `Promise<boolean>` | Elimina la entrada para esa request |
| `cache.keys(request?, options?)` | `Promise<Request[]>` | Lista las requests almacenadas |

## Estrategias de caché

| Estrategia | Flujo | Cuándo usar |
|---|---|---|
| Cache First | Sirve desde caché; si falla, red | Assets estáticos con hash en el nombre (CSS, JS con versión) |
| Network First | Intenta red; si falla, caché | Datos dinámicos que deben estar actualizados; fallback offline |
| Stale While Revalidate | Sirve caché inmediatamente y actualiza en background | Contenido que puede tolerar un ciclo desactualizado; buena UX |
| Cache Only | Solo caché; nunca red | Assets que se precachearon en `install` y no cambian |
| Network Only | Solo red; nunca caché | Peticiones no cacheables (POST, analytics) |

## Precacheo en el evento `install`

```js
// service-worker.js
const CACHE_NAME = "shell-v1";
const ASSETS = ["/", "/index.html", "/app.js", "/styles/main.css", "/logo.svg"];

self.addEventListener("install", e => {
  e.waitUntil(
    caches.open(CACHE_NAME).then(cache => cache.addAll(ASSETS))
  );
  self.skipWaiting(); // activa el SW inmediatamente sin esperar a que se cierre la pestaña
});
```

`e.waitUntil()` extiende la vida del evento hasta que la Promise se resuelva; si `addAll` falla (una URL devuelve error), el Service Worker no se instala.

## Stale-while-revalidate en el evento `fetch`

```js
// service-worker.js
self.addEventListener("fetch", e => {
  e.respondWith(
    caches.open("dynamic-v1").then(async cache => {
      const cached = await cache.match(e.request);

      const fetchPromise = fetch(e.request).then(networkResponse => {
        cache.put(e.request, networkResponse.clone()); // clonar: el body es un ReadableStream
        return networkResponse;
      });

      return cached ?? fetchPromise; // sirve caché si existe; si no, espera la red
    })
  );
});
```

> [!warning]
> `Response.body` es un `ReadableStream` de un solo uso. Antes de guardar una respuesta en caché con `cache.put()`, es obligatorio llamar a `response.clone()` — de lo contrario la respuesta queda consumida y el navegador recibe un body vacío.

## Limpieza de cachés antiguas

Una gestión correcta de versiones requiere eliminar las cachés de versiones anteriores en el evento `activate`:

```js
self.addEventListener("activate", e => {
  const CURRENT_CACHES = ["shell-v1", "dynamic-v1"];

  e.waitUntil(
    caches.keys().then(names =>
      Promise.all(
        names
          .filter(name => !CURRENT_CACHES.includes(name))
          .map(name => caches.delete(name))
      )
    )
  );
  self.clients.claim(); // toma control de las pestañas abiertas sin recargar
});
```

## Cómo funciona por dentro

La Cache API almacena los datos en el sistema de archivos del origen, gestionado por el navegador bajo la misma cuota que IndexedDB y otros mecanismos de almacenamiento persistente. Las entradas son pares (`Request`, `Response`) donde ambas se serializan con el algoritmo structured clone. A diferencia de la caché HTTP del navegador, la Cache API está bajo control total del Service Worker: el código decide qué se cachea, durante cuánto tiempo y con qué estrategia, sin depender de los headers `Cache-Control` del servidor (aunque estos siguen siendo válidos en las respuestas almacenadas).

> [!tip]
> Incluir la versión en el nombre de la caché (`"shell-v2"`, `"assets-2024-11"`) es la forma más sencilla de forzar una actualización: el evento `activate` detecta nombres que no están en la lista actual y los elimina automáticamente.

## Notas relacionadas

- [[03 IndexedDB/index|IndexedDB]] — almacenamiento de datos estructurados JS (distinto de pares Request/Response)
- [[index|09 Almacenamiento en Cliente]] — visión comparativa de todos los mecanismos de almacenamiento
