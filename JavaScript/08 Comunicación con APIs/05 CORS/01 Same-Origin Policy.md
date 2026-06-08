---
title: Same-Origin Policy — restricción de acceso entre orígenes
aliases:
  - Same-Origin Policy
  - SOP
  - política del mismo origen
tags:
  - javascript
  - red
draft: false
---

# Same-Origin Policy

> [!definicion]
> La **Same-Origin Policy (SOP)** es una restricción de seguridad del navegador que impide que código JS de un origen lea recursos de otro origen distinto. Dos URLs tienen el **mismo origen** si y solo si coinciden los tres componentes: **protocolo + host + puerto**. El cambio en cualquiera de los tres constituye un origen diferente.

```text
https://app.ejemplo.com:443/ruta/pagina

protocolo → https
host      → app.ejemplo.com
puerto    → 443  (implícito en https)
```

## Tabla de ejemplos de origen

| URL A | URL B | Mismo origen | Motivo |
|---|---|---|---|
| `https://app.com/pagina` | `https://app.com/otro` | Sí | Solo difiere la ruta |
| `https://app.com` | `http://app.com` | No | Protocolo diferente |
| `https://app.com` | `https://api.app.com` | No | Subdominio diferente |
| `https://app.com` | `https://app.com:8080` | No | Puerto diferente |
| `http://localhost:3000` | `http://localhost:4000` | No | Puerto diferente |
| `https://app.com` | `https://app.com:443` | Sí | Puerto implícito igual |

## Qué bloquea la SOP

El navegador impide al JS del origen A:

- Leer la **respuesta de `fetch`/XHR** a un recurso de origen B (la petición llega al servidor, pero el browser no entrega la respuesta al script).
- Acceder al **DOM de un `<iframe>`** que carga una página de otro origen.
- Leer `localStorage`, `sessionStorage` o cookies de otro origen.
- Invocar métodos en un `ServiceWorker` de otro scope de origen.

## Qué permite la SOP por defecto

La política permite cargar recursos de otros orígenes mediante **tags HTML**, pero el JS no puede leer su contenido:

```html
<!-- Permitido — el browser carga el recurso, pero JS no puede inspeccionar su contenido -->
<img src="https://cdn.ejemplo.com/foto.jpg">
<script src="https://cdn.ejemplo.com/libreria.js"></script>
<link rel="stylesheet" href="https://cdn.ejemplo.com/estilos.css">
<video src="https://media.ejemplo.com/video.mp4"></video>

<!-- Bloqueado — JS no puede leer datos del iframe de otro origen -->
<iframe src="https://banco.com/cuenta"></iframe>
```

La regla práctica: **incrustar** recursos es libre; **leer su contenido con JS** está restringido.

## Por qué existe

La SOP protege principalmente contra dos vectores:

**CSRF pasivo** — sin SOP, un script malicioso en `https://atacante.com` podría hacer `fetch('https://banco.com/movimientos')` con las cookies del usuario (que el browser envía automáticamente) y leer la respuesta con los datos bancarios.

**Lectura de datos privados entre pestañas** — sin SOP, cualquier pestaña abierta podría leer el DOM de otra pestaña con sesión activa en otro servicio.

## Relación con CORS

La SOP es la restricción; [[index|CORS]] es el mecanismo estándar que permite al servidor relajarla de forma controlada y explícita. Sin CORS (ni otras alternativas como JSONP o proxies), la SOP bloquea toda lectura cross-origin en JS.

> [!tip]
> En desarrollo local, el puerto forma parte del origen: `http://localhost:3000` y `http://localhost:8080` son orígenes distintos aunque el host sea el mismo `localhost`. La solución habitual es configurar CORS en el servidor de desarrollo o usar un proxy del bundler (Vite, webpack-dev-server).

> [!warning]
> La SOP opera en el **navegador**, no en el servidor. El servidor recibe la petición igualmente — lo que bloquea el browser es la entrega de la respuesta al script. Esto significa que peticiones mutantes (POST que crea datos, DELETE que borra) pueden ejecutarse en el servidor aunque el JS no pueda leer la respuesta. El preflight CORS existe precisamente para mitigar este riesgo.

## Notas relacionadas

- [[index|CORS]] — mecanismo para relajar la SOP de forma controlada
- [[02 Peticiones Simples vs Preflight|Peticiones Simples vs Preflight]] — cuándo el browser pide permiso antes de enviar
- [[04 Credenciales|Credenciales]] — cookies cross-origin y sus restricciones adicionales
