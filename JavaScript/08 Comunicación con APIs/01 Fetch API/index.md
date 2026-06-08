---
title: Fetch API — peticiones HTTP basadas en Promises
aliases:
  - Fetch API
  - fetch
tags:
  - javascript
  - api/web
  - red
draft: false
---

# Fetch API

> [!definicion]
> La **Fetch API** es la interfaz moderna del navegador para realizar peticiones HTTP. Reemplaza a [[02 XMLHttpRequest (legado)/index|XMLHttpRequest]] con una interfaz basada en [[../07 Programación Asíncrona/05 Promesas/index|Promesas]], diseñada para ser componible con `async/await` y el ecosistema de streams del navegador. El punto de entrada es la función global `fetch(url, opciones?)`.

El uso central: obtener datos de una API REST y deserializarlos.

```js
const res = await fetch('/api/productos');
if (!res.ok) throw new Error(`HTTP ${res.status}`);
const productos = await res.json();
```

## Bloques de esta sección

- [[01 Sintaxis Básica|Sintaxis Básica]] — `fetch(url)`, Promise<Response>, `res.ok`, `res.status`, patrón async/await.
- [[02 Configuración (method, headers, body)|Configuración (method, headers, body)]] — segundo argumento de fetch, tipos de body, recipes POST/PUT.
- [[03 mode, credentials, cache|mode, credentials, cache]] — CORS, cookies, control de caché y redirecciones.
- [[04 Objeto Response|Objeto Response]] — propiedades de la respuesta, Headers, body como ReadableStream, clone.
- [[05 Procesar Respuesta (json, text, blob)|Procesar Respuesta (json, text, blob)]] — métodos del body, descarga de archivos, streaming.
- [[06 Manejo de Errores|Manejo de Errores]] — errores de red vs HTTP vs parseo, función wrapper, retry con backoff.
- [[07 AbortController|AbortController]] — cancelación de peticiones, AbortSignal.timeout, cleanup en React/Vue.

## Comparativa con XMLHttpRequest

| Característica | Fetch API | XMLHttpRequest |
|---|---|---|
| Interfaz | Basada en Promises | Basada en callbacks y eventos |
| Streaming del body | Sí (ReadableStream) | No |
| Service Workers | Sí (interceptable) | No |
| CORS | Nativo, configurable | Sí pero más verboso |
| Cancelación | AbortController | `xhr.abort()` |
| Progress de upload | No nativo | Sí (`xhr.upload.onprogress`) |
| Soporte legado | IE no soportado | Soporta IE |

## Cuándo NO usar Fetch directamente

- Cuando se necesita progreso de carga (upload progress) en el navegador: usar `XMLHttpRequest` o una librería como `axios`.
- Cuando el proyecto ya usa una capa de cliente HTTP con interceptores centralizados (`axios`, `ky`, `wretch`): Fetch resulta más verboso para ese patrón.
- Para aplicaciones Node.js anteriores a v18: Fetch no estaba disponible de forma nativa (requería `node-fetch`).

## Notas relacionadas

- [[../07 Programación Asíncrona/05 Promesas/index|Promesas]] — el mecanismo subyacente de fetch
- [[../07 Programación Asíncrona/06 Async - Await/index|Async/Await]] — sintaxis para consumir fetch sin .then()
- [[02 XMLHttpRequest (legado)/index|XMLHttpRequest]] — alternativa legada
- [[03 JSON/index|JSON]] — formato de datos más común en respuestas fetch
