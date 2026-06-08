---
title: CORS — cabeceras del servidor y del cliente
aliases:
  - Access-Control-Allow-Origin
  - cabeceras CORS
  - CORS headers
tags:
  - javascript
  - red
draft: false
---

# Cabeceras CORS

> [!definicion]
> CORS opera mediante un protocolo de cabeceras HTTP. El **cliente** envía automáticamente `Origin` en peticiones cross-origin; el **servidor** responde con un conjunto de cabeceras `Access-Control-*` que autorizan o deniegan el acceso. El navegador compara origen y cabeceras antes de entregar la respuesta al JS. Las cabeceras ausentes equivalen a denegación.

## Cabeceras del servidor (respuesta)

### `Access-Control-Allow-Origin`

La cabecera fundamental. Indica qué orígenes pueden leer la respuesta.

```http
Access-Control-Allow-Origin: *
Access-Control-Allow-Origin: https://app.ejemplo.com
```

- `*` — cualquier origen puede leer la respuesta. **No compatible con credenciales** (`credentials: "include"`).
- Origen específico — solo ese origen puede leer. Debe coincidir exactamente (protocolo + host + puerto).
- El servidor debe variar esta cabecera dinámicamente si sirve a múltiples orígenes autorizados; en ese caso incluir también `Vary: Origin` para que las cachés intermedias no sirvan respuestas incorrectas.

### `Access-Control-Allow-Methods`

Solo relevante en la respuesta a un preflight (`OPTIONS`). Lista los métodos HTTP que el servidor acepta para peticiones cross-origin.

```http
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, PATCH
```

### `Access-Control-Allow-Headers`

Solo en respuesta a preflight. Lista las cabeceras de petición que el servidor acepta. Debe incluir cada cabecera personalizada que el cliente envíe.

```http
Access-Control-Allow-Headers: Content-Type, Authorization, X-Request-ID
```

### `Access-Control-Max-Age`

Tiempo en segundos que el browser puede cachear el resultado del preflight para ese recurso. Evita un `OPTIONS` por cada petición.

```http
Access-Control-Max-Age: 86400
```

### `Access-Control-Expose-Headers`

Por defecto, el JS del cliente solo puede leer un subconjunto de cabeceras de respuesta (las "CORS-safelisted": `Cache-Control`, `Content-Language`, `Content-Length`, `Content-Type`, `Expires`, `Last-Modified`, `Pragma`). Para exponer otras cabeceras al script, el servidor las lista aquí.

```http
Access-Control-Expose-Headers: X-Total-Count, X-Request-ID, Link
```

```js
const res = await fetch('https://api.ejemplo.com/usuarios?page=2');
const total = res.headers.get('X-Total-Count'); // solo funciona si está en Expose-Headers
```

### `Access-Control-Allow-Credentials`

Solo necesaria cuando el cliente envía `credentials: "include"`. Ver [[04 Credenciales|Credenciales]].

```http
Access-Control-Allow-Credentials: true
```

## Tabla resumen de cabeceras del servidor

| Cabecera | Cuándo se envía | Valor típico |
|---|---|---|
| `Access-Control-Allow-Origin` | Siempre (simple y preflight) | `*` o un origen específico |
| `Access-Control-Allow-Methods` | Solo en respuesta a preflight | `GET, POST, PUT, DELETE` |
| `Access-Control-Allow-Headers` | Solo en respuesta a preflight | `Content-Type, Authorization` |
| `Access-Control-Max-Age` | Solo en respuesta a preflight | `86400` (1 día) |
| `Access-Control-Expose-Headers` | Siempre (cuando aplica) | `X-Total-Count, Link` |
| `Access-Control-Allow-Credentials` | Solo con credenciales | `true` |
| `Vary: Origin` | Con Allow-Origin dinámico | `Origin` |

## Cabecera del cliente (petición)

El browser envía automáticamente `Origin` en toda petición cross-origin. El JS no puede establecer ni leer esta cabecera manualmente — es una "cabecera prohibida".

```http
Origin: https://app.ejemplo.com
```

En el preflight, el browser añade también:

```http
Access-Control-Request-Method: POST
Access-Control-Request-Headers: content-type, authorization
```

Estas cabeceras describen la petición real que quiere enviar, para que el servidor pueda aprobarla o denegarla antes.

## Errores CORS comunes

| Error | Causa | Solución |
|---|---|---|
| "No 'Access-Control-Allow-Origin'" | Cabecera ausente en la respuesta | El servidor debe enviarla siempre |
| "has been blocked" con `*` y cookies | `*` no es compatible con credenciales | Especificar el origen exacto |
| Preflight devuelve 404 | El servidor no maneja `OPTIONS` en esa ruta | Añadir handler para `OPTIONS` |
| Cabecera personalizada bloqueada | No incluida en `Allow-Headers` | Añadirla al valor de `Allow-Headers` |
| Cabecera de respuesta no disponible | No listada en `Expose-Headers` | Añadirla al valor de `Expose-Headers` |

## Cómo funciona por dentro

El browser opera como intermediario que aplica la política CORS. Al recibir la respuesta del servidor, extrae `Access-Control-Allow-Origin` y la compara con el `Origin` de la petición. Si no coincide (o si la cabecera está ausente), el browser descarta la respuesta y rechaza la Promise con un `TypeError`. El servidor recibió y procesó la petición; el rechazo ocurre únicamente en el lado del cliente.

> [!tip]
> Cuando se sirve a múltiples orígenes autorizados (staging, producción, distintos frontends), la estrategia correcta es mantener una lista blanca en el servidor: si el `Origin` de la petición está en la lista, responder con `Access-Control-Allow-Origin: <ese origen>` y `Vary: Origin`. Usar `*` solo en APIs verdaderamente públicas sin estado de usuario.

> [!warning]
> Usar `Access-Control-Allow-Origin: *` con `Access-Control-Allow-Credentials: true` es una configuración inválida: el browser la rechaza. Para peticiones con credenciales, `Allow-Origin` debe ser un origen específico.

## Notas relacionadas

- [[02 Peticiones Simples vs Preflight|Peticiones Simples vs Preflight]] — cuándo el servidor recibe un OPTIONS y qué debe responder
- [[04 Credenciales|Credenciales]] — restricciones adicionales con cookies y autenticación
- [[01 Same-Origin Policy|Same-Origin Policy]] — la política que estas cabeceras relajan
