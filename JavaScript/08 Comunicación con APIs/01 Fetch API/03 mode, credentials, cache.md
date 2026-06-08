---
title: fetch — mode, credentials, cache, redirect
aliases:
  - fetch mode
  - fetch credentials
  - fetch cache
  - fetch CORS
tags:
  - javascript
  - api/funcion
  - red
objeto: global
tipo: funcion
retorna: Promise<Response>
muta: false
asincrono: true
draft: false
---

# `fetch` — mode, credentials, cache

> [!definicion]
> Las opciones `mode`, `credentials`, `cache` y `redirect` del objeto `RequestInit` controlan el comportamiento de seguridad y caché de la petición. Son relevantes en escenarios de producción: CORS entre dominios, sesiones autenticadas con cookies y control fino del comportamiento de caché del navegador.

```js
const res = await fetch('/api/datos', {
  mode: 'cors',
  credentials: 'include',
  cache: 'no-cache',
});
```

## `mode`

Controla la política CORS aplicada a la petición.

| Valor | Comportamiento | Cuándo usar |
|---|---|---|
| `"cors"` | Default. Verifica CORS; el servidor debe devolver las cabeceras `Access-Control-Allow-*` adecuadas | APIs externas que soportan CORS |
| `"no-cors"` | Petición opaca: se envía, pero la respuesta es inutilizable desde JS (`type: "opaque"`, body vacío) | Cargar recursos externos sin necesitar la respuesta (pixel de tracking, prefetch) |
| `"same-origin"` | Solo permite peticiones al mismo origen; lanza `TypeError` si el recurso es de otro origen | Garantizar que nunca se hagan peticiones cross-origin accidentalmente |
| `"navigate"` | Solo para navegación del browser (no usar en código) | Interno del navegador |

> [!warning]
> `"no-cors"` no es una forma de saltarse CORS. La respuesta opaca llega al navegador, pero JS no puede leer su body, sus headers ni su status. Solo sirve para efectos secundarios que no requieren inspeccionar la respuesta.

## `credentials`

Controla si se envían cookies, cabeceras de autenticación HTTP y certificados TLS de cliente con la petición.

| Valor | Comportamiento | Cuándo usar |
|---|---|---|
| `"omit"` | Nunca envía credenciales | Default efectivo para `mode: "cors"` |
| `"same-origin"` | Envía credenciales solo al mismo origen | Sesiones en aplicaciones de un solo dominio |
| `"include"` | Siempre envía credenciales, incluso cross-origin | Autenticación con cookies en APIs cross-origin |

Para que `credentials: "include"` funcione en cross-origin, el servidor debe responder con `Access-Control-Allow-Credentials: true` y `Access-Control-Allow-Origin` con el origen explícito (no `*`).

```js
// API cross-origin con autenticación por cookie de sesión
const res = await fetch('https://api.ejemplo.com/perfil', {
  credentials: 'include',
});
```

## `cache`

Controla cómo la petición interactúa con la caché HTTP del navegador.

| Valor | Comportamiento | Cuándo usar |
|---|---|---|
| `"default"` | Comportamiento estándar del navegador según las cabeceras HTTP (`Cache-Control`, `ETag`, etc.) | La mayoría de casos |
| `"no-store"` | Ignora la caché completamente: ni lee ni escribe. Siempre va al servidor | Datos muy dinámicos o sensibles (saldos, tiempo real) |
| `"no-cache"` | Siempre revalida con el servidor (conditional request con `ETag`/`Last-Modified`), pero puede usar la caché si el servidor confirma que no cambió | Datos que cambian frecuentemente pero pueden cachearse |
| `"reload"` | Ignora la caché al leer, pero escribe el resultado en la caché | Forzar una actualización y guardar el resultado |
| `"force-cache"` | Usa la caché aunque esté caducada; solo va al servidor si no hay entrada | Datos estáticos o de bajo cambio, modo offline-first |
| `"only-if-cached"` | Solo usa la caché; falla si no hay entrada (requiere `mode: "same-origin"`) | Comprobaciones offline |

```js
// Datos de tiempo real — nunca cachear
const res = await fetch('/api/cotizacion-bitcoin', { cache: 'no-store' });

// Datos que cambian, validar con el servidor
const res = await fetch('/api/catalogo', { cache: 'no-cache' });
```

## `redirect`

Controla el comportamiento ante redirecciones HTTP (301, 302, 307, 308).

| Valor | Comportamiento |
|---|---|
| `"follow"` | Default. Sigue redirecciones automáticamente |
| `"error"` | Lanza `TypeError` si hay redirección |
| `"manual"` | Devuelve la respuesta opaca de redirección sin seguirla (para Service Workers) |

## `referrerPolicy`

Controla la cabecera `Referer` enviada con la petición. Valores comunes: `"no-referrer"` (no envía), `"strict-origin-when-cross-origin"` (default del navegador), `"unsafe-url"` (siempre envía URL completa).

```js
const res = await fetch('/api/datos', {
  referrerPolicy: 'no-referrer', // no expone la URL de origen al servidor
});
```

## Cómo funciona por dentro

`mode` y `credentials` son procesados por el algoritmo CORS del navegador antes de enviar la petición. Si `mode: "cors"` y el servidor no devuelve `Access-Control-Allow-Origin` con el origen correcto, el navegador bloquea la respuesta y la Promise rechaza con `TypeError`. El bloqueo ocurre en el navegador, no en el servidor (el servidor sí recibió la petición).

`cache` se procesa en la capa del service worker / caché HTTP del navegador antes de que la petición salga a la red, siguiendo la especificación Fetch de WHATWG.

> [!tip]
> Para APIs de datos dinámicos en producción, la combinación `cache: 'no-store'` con `credentials: 'same-origin'` es un punto de partida seguro. Agregar `credentials: 'include'` solo cuando la autenticación sea mediante cookies HttpOnly.

> [!warning]
> `cache: 'no-cache'` no significa "sin caché" — significa "revalidar siempre". Para eliminar completamente la caché, usar `cache: 'no-store'`. La confusión entre ambos es frecuente y puede provocar respuestas cacheadas inesperadas.

## Notas relacionadas

- [[01 Sintaxis Básica|Sintaxis Básica]] — firma básica y opciones mínimas
- [[02 Configuración (method, headers, body)|Configuración (method, headers, body)]] — method, headers, body
- [[04 Objeto Response|Objeto Response]] — `res.type` refleja el resultado del modo CORS
- [[07 AbortController|AbortController]] — opción `signal` para cancelación de peticiones
