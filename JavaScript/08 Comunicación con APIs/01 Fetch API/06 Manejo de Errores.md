---
title: fetch — manejo de errores HTTP, de red y de parseo
aliases:
  - fetch errores
  - fetch error handling
  - TypeError Failed to fetch
tags:
  - javascript
  - api/concepto
  - red
objeto: global
tipo: concepto
draft: false
order: 6
---

# Manejo de Errores en `fetch`

> [!definicion]
> `fetch` distingue tres categorías de error con comportamientos distintos: **errores de red** (la Promise rechaza con `TypeError`), **errores HTTP** (la Promise se cumple pero `res.ok === false` y `res.status` es 4xx/5xx), y **errores de parseo** (el método del body lanza `SyntaxError`). El manejo correcto requiere comprobar explícitamente `res.ok` y capturar tanto rechazos como excepciones síncronas del parseo.

```js
try {
  const res = await fetch('/api/datos');
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  const data = await res.json();
} catch (err) {
  if (err.name === 'AbortError') return; // cancelación intencional
  console.error('Fallo en fetch:', err.message);
}
```

## Las tres categorías de error

### 1. Errores de red — rechazo de la Promise

Ocurren cuando el navegador no puede ni establecer la conexión. La Promise rechaza con `TypeError`.

| Causa | Mensaje típico |
|---|---|
| Sin conexión a internet | `TypeError: Failed to fetch` |
| CORS bloqueado por el servidor | `TypeError: Failed to fetch` |
| URL inválida o esquema desconocido | `TypeError: Failed to fetch` |
| Petición abortada con `AbortController` | `AbortError: The user aborted a request` |
| Timeout de AbortSignal.timeout | `TimeoutError: signal timed out` |

```js
try {
  const res = await fetch('https://host-inexistente.local/api');
} catch (err) {
  // err es TypeError, no un objeto Response
  console.error(err.name);    // "TypeError"
  console.error(err.message); // "Failed to fetch"
}
```

### 2. Errores HTTP — `res.ok === false`

El servidor respondió, pero con un código de error (4xx o 5xx). La Promise se **cumple** con el objeto `Response`. Sin comprobación explícita, el error pasa silenciosamente.

```js
const res = await fetch('/recurso-inexistente');
// La Promise se cumplió — NO hubo rechazo
res.ok;     // false
res.status; // 404

// Error 500: el body podría ser HTML de error del servidor
const resError = await fetch('/api/operacion-que-falla');
resError.status; // 500
resError.ok;     // false
```

### 3. Errores de parseo — excepción en métodos del body

Si `res.json()` se llama sobre un body que no es JSON válido (HTML de un error 500, string vacío, XML), lanza `SyntaxError`. Esto ocurre aunque la comprobación de `res.ok` se haya omitido.

```js
const res = await fetch('/api/error-inesperado');
// Servidor devolvió HTTP 500 con body: "<html>Internal Server Error</html>"
try {
  const data = await res.json(); // SyntaxError: Unexpected token '<'
} catch (err) {
  console.error(err.name); // "SyntaxError"
}
```

## Función wrapper robusta

```js
async function apiFetch(url, opciones = {}) {
  const res = await fetch(url, opciones); // TypeError si hay fallo de red

  if (!res.ok) {
    // Intentar leer el body del error para incluir en el mensaje
    const texto = await res.text().catch(() => '(sin cuerpo)');
    throw new Error(`HTTP ${res.status} ${res.statusText}: ${texto.slice(0, 200)}`);
  }

  // Deserializar solo si hay contenido
  if (res.status === 204 || res.headers.get('content-length') === '0') {
    return null;
  }

  return res.json();
}

// Uso
try {
  const usuario = await apiFetch('/api/usuarios/42');
} catch (err) {
  if (err.name === 'AbortError') return;
  // err.message incluye el status HTTP o el mensaje de red
  mostrarErrorAlUsuario(err.message);
}
```

## Retry con backoff exponencial

```js
async function fetchConRetry(url, opciones = {}, reintentos = 3, espera = 500) {
  for (let intento = 0; intento <= reintentos; intento++) {
    try {
      const res = await fetch(url, opciones);

      // No reintentar en errores del cliente (4xx)
      if (res.status >= 400 && res.status < 500) {
        throw new Error(`HTTP ${res.status} — no reintentable`);
      }

      if (!res.ok) throw new Error(`HTTP ${res.status}`);
      return res.json();

    } catch (err) {
      // No reintentar si es cancelación o error del cliente
      if (err.name === 'AbortError' || err.message.includes('no reintentable')) throw err;

      if (intento === reintentos) throw err;

      // Espera exponencial: 500ms, 1000ms, 2000ms…
      await new Promise(r => setTimeout(r, espera * 2 ** intento));
    }
  }
}
```

## Distinguir `AbortError` vs error de red vs error HTTP

```js
async function peticionSegura(url, signal) {
  try {
    const res = await fetch(url, { signal });
    if (!res.ok) {
      // Error HTTP: devolver un resultado de error estructurado
      return { ok: false, status: res.status, data: null };
    }
    const data = await res.json();
    return { ok: true, status: res.status, data };

  } catch (err) {
    if (err.name === 'AbortError' || err.name === 'TimeoutError') {
      // Cancelación intencional — no es un error a reportar
      return { ok: false, status: 0, data: null, abortado: true };
    }
    // TypeError de red u otro error inesperado
    console.error('Error de red:', err.message);
    return { ok: false, status: 0, data: null };
  }
}
```

## Cómo funciona por dentro

El motor de red del navegador comunica los fallos de red al runtime de JS como rechazos de la Promise, sin pasar por el objeto `Response`. Los errores HTTP en cambio llegan como respuestas HTTP válidas — el browser no distingue semánticamente entre un 200 y un 404 desde el protocolo HTTP. Por eso `fetch` los trata igual: la Promise se cumple con el `Response` y es responsabilidad del código de aplicación interpretar `status` y `ok`.

> [!tip]
> Centralizar el manejo de errores de fetch en una función wrapper (`apiFetch`, `httpClient`) en lugar de repetir `if (!res.ok)` en cada llamada. Así se puede añadir logging, telemetría, refresco de tokens o retry de forma transparente.

> [!warning]
> El error `SyntaxError: Unexpected token '<'` al llamar a `res.json()` casi siempre indica que el servidor devolvió HTML (página de error o de login) en lugar de JSON. La causa suele ser un error 500 o una redirección a la página de login por sesión expirada. Comprobar siempre `res.ok` y `res.headers.get('content-type')` antes de llamar a `res.json()`.

## Notas relacionadas

- [[01 Sintaxis Básica|Sintaxis Básica]] — `res.ok`, `res.status`, cuándo rechaza la Promise
- [[05 Procesar Respuesta (json, text, blob)|Procesar Respuesta]] — SyntaxError en res.json()
- [[07 AbortController|AbortController]] — AbortError y TimeoutError en el catch
