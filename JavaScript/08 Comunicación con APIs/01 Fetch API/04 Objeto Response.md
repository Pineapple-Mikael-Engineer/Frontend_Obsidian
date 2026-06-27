---
title: Response — objeto de respuesta HTTP de fetch
aliases:
  - Response
  - objeto Response
  - fetch Response
tags:
  - javascript
  - api/clase
  - red
objeto: Response
tipo: clase
retorna: Response
muta: false
asincrono: false
draft: false
order: 4
---

# Objeto `Response`

> [!definicion]
> `Response` es el objeto con el que la Promise de `fetch` se cumple. Representa la respuesta HTTP completa: status, headers y body. El body es un `ReadableStream` y solo puede consumirse **una vez** — si se llama a `res.json()` no se puede volver a llamar a `res.text()` sobre el mismo objeto. Para leer el body varias veces, usar `res.clone()` antes de consumirlo.

```js
const res = await fetch('/api/usuarios/1');

console.log(res.ok);         // true
console.log(res.status);     // 200
console.log(res.statusText); // "OK"
console.log(res.url);        // "https://ejemplo.com/api/usuarios/1"
console.log(res.bodyUsed);   // false — aún no se ha leído

const data = await res.json();
console.log(res.bodyUsed);   // true — body consumido
```

## Propiedades de instancia

| Propiedad | Tipo | Descripción |
|---|---|---|
| `ok` | `boolean` | `true` si `status` está en 200–299 |
| `status` | `number` | Código HTTP (200, 301, 404, 500…) |
| `statusText` | `string` | Texto del status ("OK", "Not Found"…) |
| `headers` | `Headers` | Cabeceras de la respuesta; instancia iterable de `Headers` |
| `url` | `string` | URL final tras redirecciones |
| `redirected` | `boolean` | `true` si la respuesta llegó tras una o más redirecciones |
| `type` | `string` | Tipo de respuesta según CORS (ver abajo) |
| `bodyUsed` | `boolean` | `true` si el body ya fue consumido por un método de lectura |
| `body` | `ReadableStream o null` | Stream del body crudo; `null` si no hay body |

## `Response.type`

Refleja el resultado del modo CORS de la petición.

| Valor | Origen | Descripción |
|---|---|---|
| `"basic"` | Mismo origen | Respuesta normal con todos los headers accesibles |
| `"cors"` | Cross-origin con CORS permitido | Respuesta con headers filtrados por el servidor |
| `"opaque"` | `mode: "no-cors"` | Body vacío, status 0, headers vacíos — inutilizable desde JS |
| `"error"` | Fallo de red | Generado por `Response.error()`, status 0 |

## `Response.headers` — instancia de `Headers`

Las cabeceras de la respuesta son accesibles a través de la instancia `Headers`. En respuestas CORS de tipo `"cors"`, solo están disponibles las cabeceras que el servidor expone en `Access-Control-Expose-Headers`.

```js
const res = await fetch('/api/datos');

res.headers.get('Content-Type');    // "application/json; charset=utf-8"
res.headers.get('X-Request-Id');    // "abc-123" (si el servidor la expone)
res.headers.has('Authorization');   // false (no enviada en la respuesta)

// Iterar todas las cabeceras disponibles
for (const [nombre, valor] of res.headers) {
  console.log(`${nombre}: ${valor}`);
}
```

## `Response.clone()` — leer el body más de una vez

El body de un `Response` es un stream que se consume con la primera lectura. `clone()` crea una copia del objeto `Response` con un nuevo stream, permitiendo lecturas independientes. Debe llamarse **antes** de consumir el body original.

```js
const res = await fetch('/api/datos');
const copia = res.clone(); // clonar antes de leer

// Leer el original como JSON
const datos = await res.json();

// Leer la copia para debug o cache
const texto = await copia.text();
console.log('[DEBUG raw]', texto);
```

> [!info]
> `clone()` es especialmente útil en Service Workers, donde se puede interceptar una petición, clonar la respuesta para guardarla en caché y pasar la respuesta original al cliente — todo desde el mismo objeto `Response`.

```js
// Service Worker — cache + respuesta al cliente
self.addEventListener('fetch', event => {
  event.respondWith(
    fetch(event.request).then(res => {
      const cacheRes = res.clone();
      caches.open('v1').then(cache => cache.put(event.request, cacheRes));
      return res; // el original va al cliente
    })
  );
});
```

## Cómo funciona por dentro

Cuando `fetch` resuelve la Promise, el body de la respuesta aún no está completamente descargado en memoria — es un `ReadableStream` que fluye desde la red. Los métodos como `res.json()` o `res.text()` leen ese stream de forma asíncrona y lo acumulan en memoria. Por esto son métodos async que retornan Promises, y por esto el body solo puede consumirse una vez sin `clone()`.

> [!tip]
> Para comprobar el tipo de contenido antes de deserializar, usar `res.headers.get('content-type')`. Si el servidor devuelve `text/html` donde se esperaba `application/json`, llamar a `res.json()` lanzará `SyntaxError`. La comprobación del Content-Type evita errores de parseo confusos.

> [!warning]
> Intentar leer el body de un `Response` con `bodyUsed === true` lanza `TypeError: body stream already read`. Si una función auxiliar ya llamó a `res.json()`, el objeto ya está consumido. Clonar antes de pasar el objeto o leer el body una sola vez y pasar el resultado deserializado.

## Notas relacionadas

- [[01 Sintaxis Básica|Sintaxis Básica]] — `res.ok`, `res.status`, patrón de comprobación
- [[05 Procesar Respuesta (json, text, blob)|Procesar Respuesta]] — métodos para consumir el body
- [[06 Manejo de Errores|Manejo de Errores]] — uso de `res.ok` y `res.status` en manejo de errores HTTP
