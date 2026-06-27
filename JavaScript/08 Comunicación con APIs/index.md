---
title: Comunicación con APIs — peticiones HTTP y tiempo real
aliases:
  - Comunicación con APIs
  - HTTP JavaScript
  - APIs web
tags:
  - javascript
  - red
draft: false
order: 8
---

# Comunicación con APIs

> [!definicion]
> La **comunicación con APIs** agrupa los mecanismos por los que el frontend obtiene datos externos o los envía a servidores sin recargar la página. Abarca desde el modelo clásico request-response (XHR, Fetch) hasta canales persistentes bidireccionales (WebSockets), pasando por los formatos de datos y las restricciones de seguridad que gobiernan las peticiones entre orígenes.

La evolución del stack: `XMLHttpRequest` (1999, callbacks) → `Fetch API` (2015, Promises) → `WebSockets` (tiempo real bidireccional). En proyectos modernos, Fetch con `async/await` cubre la mayoría de los casos; WebSocket entra cuando el servidor necesita empujar datos sin que el cliente lo solicite.

```js
// Fetch — modelo request-response
const datos = await fetch('/api/items').then(r => r.json());

// WebSocket — canal persistente
const ws = new WebSocket('wss://api.ejemplo.com/live');
ws.addEventListener('message', (e) => actualizarUI(JSON.parse(e.data)));
```

## Secciones

| N.º | Sección | Propósito |
|---|---|---|
| 01 | Fetch API | Peticiones HTTP modernas basadas en Promises; `Response`, streams, `AbortController` |
| 02 | XMLHttpRequest (legado) | API de peticiones con callbacks; referencia para código existente |
| 03 | JSON | Formato de intercambio dominante; `JSON.stringify`, `JSON.parse`, replacer, reviver |
| 04 | FormData | Envío de formularios y archivos binarios con `multipart/form-data` |
| 05 | CORS | Restricciones de origen cruzado, cabeceras preflight, configuración del servidor |
| 06 | WebSockets | Conexión TCP persistente bidireccional; eventos, reconexión, casos de tiempo real |

- [[01 Fetch API/index|Fetch API]] — `fetch(url, opciones)`, objeto `Response`, manejo de errores, streaming, cancelación con `AbortController`.
- [[02 XMLHttpRequest (legado)/index|XMLHttpRequest]] — `open()`, `send()`, eventos `readystatechange`, `progress`; alternativa cuando se necesita progress de upload.
- [[03 JSON/index|JSON]] — `JSON.stringify(value, replacer, space)`, `JSON.parse(text, reviver)`, tipos soportados, casos límite.
- [[04 FormData/index|FormData]] — construcción desde formulario o manual, `append`, `get`, `set`; envío con Fetch sin cabecera `Content-Type` manual.
- [[05 CORS/index|CORS]] — modelo de origen, peticiones simples vs preflight, `Access-Control-Allow-*`, credenciales, depuración.
- [[06 WebSockets/index|WebSockets]] — `new WebSocket(url)`, `readyState`, `send()`, eventos del ciclo de vida, reconexión automática con backoff.

## Cuándo usar cada mecanismo

| Situación | Mecanismo |
|---|---|
| Consumir una API REST (GET, POST, PUT, DELETE) | Fetch API |
| Progreso de subida de archivos (upload progress) | XMLHttpRequest |
| Datos en formato texto estructurado entre cliente y servidor | JSON |
| Enviar un formulario con archivos adjuntos | FormData + Fetch |
| Petición a un dominio distinto al de la app | CORS (restricción automática, configurar en servidor) |
| El servidor debe empujar datos sin petición del cliente | WebSockets |
| Solo el servidor envía eventos (sin envío del cliente) | EventSource / Server-Sent Events |

## Notas relacionadas

- [[../07 Programación Asíncrona/index|Programación Asíncrona]] — Promises y async/await, el sustrato de Fetch
- [[../05 Manipulación del DOM/index|Manipulación del DOM]] — dónde se refleja lo obtenido de las APIs
