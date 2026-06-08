---
title: CORS — credenciales cross-origin (cookies y autenticación)
aliases:
  - CORS credenciales
  - credentials include
  - Access-Control-Allow-Credentials
tags:
  - javascript
  - red
draft: false
---

# Credenciales en CORS

> [!definicion]
> Por defecto, `fetch` no incluye cookies, cabeceras `Authorization` gestionadas por el browser ni certificados de cliente en peticiones cross-origin. Para enviar credenciales, la petición debe indicar `credentials: "include"` **y** el servidor debe responder con `Access-Control-Allow-Origin` (origen específico, no `*`) y `Access-Control-Allow-Credentials: true`. Si alguna de las dos partes falla, el browser rechaza la respuesta aunque el servidor la haya devuelto correctamente.

```js
// Petición cross-origin con cookies de sesión
const res = await fetch('https://api.ejemplo.com/perfil', {
  credentials: 'include',  // envía cookies e incluye credenciales del browser
});

if (!res.ok) throw new Error(`HTTP ${res.status}`);
const perfil = await res.json();
```

Cabeceras que el servidor debe devolver:

```http
Access-Control-Allow-Origin: https://app.ejemplo.com
Access-Control-Allow-Credentials: true
```

## Valores de `credentials`

| Valor | Comportamiento |
|---|---|
| `"omit"` | Nunca envía credenciales, incluso en peticiones del mismo origen |
| `"same-origin"` | Envía credenciales solo si el origen es el mismo (valor por defecto) |
| `"include"` | Envía siempre, incluyendo peticiones cross-origin |

El valor por defecto de `fetch` es `"same-origin"` — las cookies se envían en peticiones al mismo dominio, pero no a otros orígenes.

## Requisitos combinados

Para que una petición cross-origin con credenciales funcione, **ambas** condiciones deben cumplirse:

```js
// Cliente — fetch con credentials: "include"
const res = await fetch('https://api.ejemplo.com/datos', {
  credentials: 'include',
});
```

```http
# Servidor — ambas cabeceras obligatorias
Access-Control-Allow-Origin: https://app.ejemplo.com   ← origen específico, nunca *
Access-Control-Allow-Credentials: true
```

Si el servidor usa `Access-Control-Allow-Origin: *` pero el cliente pide `credentials: "include"`, el browser rechaza la respuesta con error CORS. Si el servidor omite `Access-Control-Allow-Credentials: true`, también rechaza.

## Caso de uso: autenticación por cookies de sesión

```js
// Login — el servidor establece una cookie HttpOnly de sesión
const loginRes = await fetch('https://api.ejemplo.com/auth/login', {
  method: 'POST',
  credentials: 'include',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ email: 'ana@ejemplo.com', password: '…' }),
});

// Petición autenticada posterior — la cookie viaja automáticamente
const datosRes = await fetch('https://api.ejemplo.com/perfil', {
  credentials: 'include',
});
const perfil = await datosRes.json();

// Logout — el servidor invalida la cookie
await fetch('https://api.ejemplo.com/auth/logout', {
  method: 'POST',
  credentials: 'include',
});
```

El flujo depende de que el servidor configure correctamente `Set-Cookie` con `SameSite=None; Secure` cuando opera en un contexto cross-origin en HTTPS.

## Alternativa: tokens Bearer en Authorization

Los tokens JWT u otros tokens opacos en la cabecera `Authorization` no requieren `credentials: "include"`. Son una cabecera personalizada que el JS gestiona explícitamente, no una credencial del browser:

```js
const token = localStorage.getItem('access_token');

const res = await fetch('https://api.ejemplo.com/datos', {
  headers: {
    Authorization: `Bearer ${token}`,
  },
  // credentials: "include" no es necesario
});
```

Esta petición sí dispara un preflight (porque `Authorization` es cabecera personalizada), pero el servidor solo necesita `Access-Control-Allow-Origin` y `Access-Control-Allow-Headers: Authorization` — no necesita `Access-Control-Allow-Credentials: true`.

## Tabla comparativa: cookies vs tokens Bearer

| Característica | Cookies de sesión | Token Bearer (Authorization) |
|---|---|---|
| Almacenamiento | Browser (HttpOnly, seguro ante XSS) | localStorage / memoria |
| Envío automático | Sí (si credentials: include) | No, el JS lo gestiona |
| Vulnerable a CSRF | Sí (mitigar con SameSite y tokens CSRF) | No (JS debe enviar el token conscientemente) |
| Requiere Allow-Credentials | Sí | No |
| Preflight necesario | Solo si hay otras cabeceras no simples | Sí siempre (Authorization es personalizada) |

## Cómo funciona por dentro

Cuando `credentials: "include"` está activo, el browser adjunta las cookies y credenciales del dominio destino (no del origen actual). Al recibir la respuesta, comprueba simultáneamente que `Access-Control-Allow-Origin` sea el origen exacto de la petición **y** que `Access-Control-Allow-Credentials: true` esté presente. Si cualquiera de las dos comprobaciones falla, descarta la respuesta completa y rechaza la Promise con `TypeError`. Este doble requisito existe para evitar que un servidor descuidado que usa `*` exponga involuntariamente datos autenticados.

> [!tip]
> Para APIs de primer partido (same-site pero cross-origin por puerto o subdominio), la opción más simple en desarrollo es un proxy en el bundler (Vite `proxy`, webpack `devServer.proxy`) que elimina el problema de origen completamente. En producción, servir el frontend y la API desde el mismo origen evita toda la complejidad de CORS con credenciales.

> [!warning]
> Las cookies cross-origin requieren `SameSite=None; Secure` en el atributo `Set-Cookie`. Sin `SameSite=None`, los browsers modernos no envían la cookie en contextos cross-site, independientemente de CORS. Sin `Secure`, la cookie con `SameSite=None` se rechaza en navegadores recientes. Ambas restricciones obligan a HTTPS en producción.

## Notas relacionadas

- [[03 Cabeceras CORS|Cabeceras CORS]] — `Access-Control-Allow-Origin` y `Access-Control-Allow-Credentials` en detalle
- [[02 Peticiones Simples vs Preflight|Peticiones Simples vs Preflight]] — el preflight con credenciales sigue las mismas reglas
- [[../01 Fetch API/03 mode, credentials, cache|mode, credentials, cache]] — opciones de fetch relacionadas
- [[01 Same-Origin Policy|Same-Origin Policy]] — la política base que motiva estos controles
