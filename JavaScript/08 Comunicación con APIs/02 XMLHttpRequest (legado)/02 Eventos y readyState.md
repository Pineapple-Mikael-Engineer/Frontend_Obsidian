---
title: XMLHttpRequest — Eventos y readyState
aliases:
  - readyState
  - onreadystatechange
  - onprogress XHR
tags:
  - javascript
  - api/concepto
  - red
objeto: XMLHttpRequest
tipo: concepto
retorna: undefined
muta: false
asincrono: true
draft: false
---

# Eventos y `readyState` — ciclo de vida de una petición XHR

> [!definicion]
> `readyState` es un entero de 0 a 4 que representa el estado de progreso de una petición XHR. Cada transición entre estados dispara el evento `readystatechange`. La API también expone eventos de más alto nivel (`onload`, `onerror`, `onprogress`, `upload.onprogress`) que simplifican los casos más comunes sin inspeccionar `readyState` directamente.

## Estados de `readyState`

| Valor | Constante | Descripción |
|---|---|---|
| 0 | `UNSENT` | Objeto creado, `open()` no llamado aún |
| 1 | `OPENED` | `open()` llamado, petición configurada |
| 2 | `HEADERS_RECEIVED` | Cabeceras de respuesta recibidas |
| 3 | `LOADING` | Body de respuesta en descarga (parcial) |
| 4 | `DONE` | Petición completada (éxito o error) |

## `onreadystatechange`

```js
xhr.onreadystatechange = function () {
  if (xhr.readyState === XMLHttpRequest.DONE) {   // === 4
    if (xhr.status >= 200 && xhr.status < 300) {
      console.log('OK:', xhr.response);
    } else {
      console.error('Error HTTP:', xhr.status);
    }
  }
};
```

Verificar siempre `readyState === 4` antes de leer `xhr.status`: los estados 2 y 3 también disparan el evento y en ellos `status` puede ser 0.

## Eventos modernos (preferidos sobre `onreadystatechange`)

| Evento | Equivalencia | Cuándo se dispara |
|---|---|---|
| `onload` | `readyState === 4` con respuesta recibida | Petición completada (cualquier status HTTP) |
| `onerror` | — | Error de red (no error HTTP) |
| `onabort` | — | `xhr.abort()` fue llamado |
| `ontimeout` | — | Se superó `xhr.timeout` |
| `onloadstart` | `readyState === 1` | La petición empieza a enviarse |
| `onloadend` | — | Después de `load`, `error`, `abort` o `timeout` |
| `onprogress` | `readyState === 3` | Datos de descarga recibidos parcialmente |

`onload` no distingue entre 200 y 404: se dispara para cualquier respuesta HTTP completa. Comprobar `xhr.status` dentro del handler.

## `ProgressEvent` — monitoreo de transferencia

Los eventos `onprogress` y `upload.onprogress` reciben un objeto `ProgressEvent` con tres propiedades clave:

| Propiedad | Tipo | Descripción |
|---|---|---|
| `e.loaded` | number | Bytes transferidos hasta el momento |
| `e.total` | number | Bytes totales (0 si no se conoce) |
| `e.lengthComputable` | boolean | `true` si `total` es fiable |

Cuando `e.lengthComputable` es `false`, el servidor no envió `Content-Length` y `e.total` es 0: el porcentaje no se puede calcular.

## Receta: barra de progreso de descarga

```js
function descargarConProgreso(url, onProgreso) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.responseType = 'blob';

    xhr.onprogress = (e) => {
      if (e.lengthComputable) {
        const pct = Math.round((e.loaded / e.total) * 100);
        onProgreso(pct);
      }
    };

    xhr.onload = () => {
      if (xhr.status === 200) resolve(xhr.response);
      else reject(new Error(`HTTP ${xhr.status}`));
    };
    xhr.onerror = () => reject(new Error('Error de red'));

    xhr.send();
  });
}

// Uso
const barra = document.querySelector('#barra');
descargarConProgreso('/archivos/video.mp4', pct => {
  barra.style.width = pct + '%';
  barra.textContent = pct + '%';
}).then(blob => {
  const url = URL.createObjectURL(blob);
  document.querySelector('video').src = url;
});
```

## Receta: barra de progreso de subida de archivo

`xhr.upload` es un objeto `XMLHttpRequestUpload` separado que expone los mismos eventos `ProgressEvent` pero para la fase de envío (subida del body):

```js
function subirArchivo(url, archivo, onProgreso) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open('POST', url);

    xhr.upload.onprogress = (e) => {
      if (e.lengthComputable) {
        const pct = Math.round((e.loaded / e.total) * 100);
        onProgreso(pct);
      }
    };

    xhr.onload = () => {
      if (xhr.status >= 200 && xhr.status < 300) resolve(xhr.response);
      else reject(new Error(`HTTP ${xhr.status}`));
    };
    xhr.onerror = () => reject(new Error('Error de red'));

    const formData = new FormData();
    formData.append('archivo', archivo);
    xhr.send(formData);
  });
}

// Uso
const input = document.querySelector('input[type="file"]');
const barra = document.querySelector('#barra-subida');

input.addEventListener('change', () => {
  subirArchivo('/api/upload', input.files[0], pct => {
    barra.style.width = pct + '%';
  }).then(() => console.log('Subida completa'));
});
```

## Comparativa XHR vs Fetch

| Característica | XHR | Fetch API |
|---|---|---|
| API base | Eventos y callbacks | Promises / async-await |
| Progreso de subida | `xhr.upload.onprogress` | No nativo |
| Progreso de descarga | `xhr.onprogress` | ReadableStream manual |
| Cancelación | `xhr.abort()` | `AbortController` |
| Streaming del body | No | Sí (`ReadableStream`) |
| Interceptable (Service Worker) | No | Sí |
| Verbosidad del código | Alta | Baja-media |
| Soporte IE | Sí | No (IE no soportado) |

## Cómo funciona por dentro

Los eventos de XHR son procesados por el **event loop** del navegador en la cola de tareas (macrotask), no como microtareas. Esto significa que `onload` se ejecuta después de que el stack de llamadas quede vacío, igual que `setTimeout`. En cambio, `.then()` de Fetch se encola como microtarea (promesa resuelta), lo que la hace ejecutarse antes que el siguiente macrotask pendiente. Para la mayoría de los casos prácticos la diferencia no es observable.

> [!tip]
> Usar `onload` + `onerror` + `ontimeout` en lugar de `onreadystatechange` reduce el ruido considerablemente: en lugar de filtrar cuatro transiciones de estado, cada evento tiene un significado semántico claro.

> [!warning]
> `xhr.upload.onprogress` solo tiene datos fiables si el servidor puede recibir el body en streaming. Algunos servidores (o proxies intermedios) almacenan el body completo antes de enviarlo al backend, lo que hace que el progreso salte de 0% a 100% de golpe aunque el browser lo envíe gradualmente.

## Notas relacionadas

- [[01 open y send|open y send]] — configuración de la petición antes de que arranquen los eventos
- [[01 Fetch API/index|Fetch API]] — alternativa moderna sin eventos de ciclo de vida explícitos
- [[../07 Programación Asíncrona/06 Async - Await/index|Async/Await]] — patrón para envolver XHR en Promises consumibles con await
