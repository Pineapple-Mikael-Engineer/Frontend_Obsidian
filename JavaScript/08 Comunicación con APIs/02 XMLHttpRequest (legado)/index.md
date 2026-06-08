---
title: XMLHttpRequest — API legada para peticiones HTTP asíncronas
aliases:
  - XHR
  - XMLHttpRequest
tags:
  - javascript
  - api/clase
  - red
objeto: XMLHttpRequest
tipo: clase
retorna: XMLHttpRequest
muta: false
asincrono: true
draft: false
---

# XMLHttpRequest (legado)

> [!definicion]
> **XMLHttpRequest (XHR)** es la API original del navegador para realizar peticiones HTTP asíncronas sin recargar la página, introducida por Microsoft en 1999 y estandarizada por el W3C. Expone un ciclo de vida basado en eventos y un estado explícito (`readyState`). Ha sido reemplazada funcionalmente por [[01 Fetch API/index|Fetch API]], pero sigue siendo relevante por tres razones: código legacy, monitoreo de progreso de subida/descarga con `ProgressEvent`, y librerías antiguas que la usan internamente.

El ciclo mínimo de una petición XHR:

```js
const xhr = new XMLHttpRequest();
xhr.open('GET', '/api/datos');
xhr.responseType = 'json';
xhr.onload = () => console.log(xhr.response);
xhr.onerror = () => console.error('Error de red');
xhr.send();
```

## Notas de esta sección

- [[01 open y send|open y send]] — ciclo de vida de la petición: `open`, `setRequestHeader`, `send`, `responseType`, `abort`, `timeout`.
- [[02 Eventos y readyState|Eventos y readyState]] — `readyState`, `onreadystatechange`, eventos modernos (`onload`, `onerror`, `onprogress`, `upload.onprogress`), `ProgressEvent`, barras de progreso.

## Por qué conocerla hoy

XHR no es un tema obsoleto a ignorar por tres motivos concretos:

1. **Código legacy activo.** Muchas bases de código de empresas medianas y grandes siguen usando XHR directamente o librerías que lo envuelven (`jQuery.ajax`, `axios` en versiones anteriores).
2. **Upload progress.** `xhr.upload.onprogress` es el único mecanismo nativo en el navegador para monitorizar el progreso de una subida de archivo. Fetch API no tiene equivalente nativo.
3. **ProgressEvent en descargas.** `xhr.onprogress` permite mostrar el porcentaje descargado de un recurso binario grande, algo que Fetch solo ofrece mediante streaming manual del `ReadableStream`.

## Comparativa rápida XHR vs Fetch

| Característica | XHR | Fetch API |
|---|---|---|
| Interfaz | Eventos + callbacks | Promises / async-await |
| Progreso de subida | Sí (`upload.onprogress`) | No nativo |
| Progreso de descarga | Sí (`onprogress`) | Solo con ReadableStream |
| Cancelación | `xhr.abort()` | AbortController |
| Streaming del body | No | Sí |
| Interceptable por SW | No | Sí |
| Verbosidad | Alta | Media |

## Notas relacionadas

- [[01 Fetch API/index|Fetch API]] — reemplazante moderno basado en Promesas
- [[03 JSON/index|JSON]] — formato habitual de las respuestas
- [[../07 Programación Asíncrona/05 Promesas/index|Promesas]] — mecanismo que XHR no usa pero Fetch sí
