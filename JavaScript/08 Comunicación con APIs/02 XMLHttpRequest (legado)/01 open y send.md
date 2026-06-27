---
title: XMLHttpRequest.open y send — preparar y enviar la petición
aliases:
  - xhr.open
  - xhr.send
  - open y send
tags:
  - javascript
  - api/metodo
  - red
objeto: XMLHttpRequest
tipo: metodo
retorna: undefined
muta: false
asincrono: true
draft: false
order: 1
---

# `open` y `send` — ciclo de vida de una petición XHR

> [!definicion]
> Toda petición XHR sigue el mismo ciclo: instanciar → `open()` (configura la petición) → `setRequestHeader()` opcional → `send()` (despacha). `open(método, url, async?)` prepara la conexión sin enviar nada; `send(body?)` la despacha. Los eventos de respuesta (o el estado `readyState`) notifican cuándo llegaron los datos.

```js
const xhr = new XMLHttpRequest();
xhr.open('GET', 'https://api.example.com/usuarios');
xhr.responseType = 'json';
xhr.onload = () => {
  if (xhr.status >= 200 && xhr.status < 300) {
    console.log(xhr.response); // ya es objeto JS (por responseType 'json')
  }
};
xhr.onerror = () => console.error('Error de red');
xhr.send();
```

## Firmas

### `xhr.open(method, url, async?, user?, password?)`

| Parámetro | Tipo | Descripción |
|---|---|---|
| `method` | string | Verbo HTTP: `'GET'`, `'POST'`, `'PUT'`, `'DELETE'`, etc. |
| `url` | string | URL absoluta o relativa del recurso |
| `async` | boolean | `true` por defecto. `false` hace la llamada síncrona (bloqueante, deprecado) |
| `user` | string? | Usuario para autenticación HTTP básica |
| `password` | string? | Contraseña para autenticación HTTP básica |

`open()` no envía nada: solo inicializa el estado interno. Si se llama `open()` sobre una petición ya enviada, la reinicia.

### `xhr.send(body?)`

| Valor de `body` | Cuándo usarlo |
|---|---|
| `null` o vacío | GET, DELETE (sin cuerpo) |
| `string` | Texto plano, formulario URL-encoded |
| `JSON.stringify(obj)` | JSON manual (requiere header `Content-Type`) |
| `FormData` | Formularios con archivos (multipart/form-data automático) |
| `Blob` / `ArrayBuffer` | Binarios, archivos |
| `URLSearchParams` | Query string como body |

## Configuración antes de `send()`

### `xhr.setRequestHeader(nombre, valor)`

Añade o acumula cabeceras. Debe llamarse **después** de `open()` y **antes** de `send()`.

```js
xhr.setRequestHeader('Content-Type', 'application/json');
xhr.setRequestHeader('Authorization', 'Bearer ' + token);
```

Algunas cabeceras son controladas por el navegador y no pueden sobrescribirse (`Host`, `Content-Length`, `Origin`, etc.).

### `xhr.responseType`

Indica el formato en que se quiere recibir `xhr.response`:

| Valor | Tipo de `xhr.response` |
|---|---|
| `""` (vacío) | string (texto plano, igual que `responseText`) |
| `"text"` | string |
| `"json"` | objeto JS (parseado automáticamente) |
| `"blob"` | `Blob` (imágenes, archivos) |
| `"arraybuffer"` | `ArrayBuffer` (binario raw) |
| `"document"` | `Document` XML/HTML parseado |

Fijar `responseType = 'json'` evita llamar `JSON.parse()` manualmente.

### `xhr.timeout`

Tiempo máximo en milisegundos antes de cancelar la petición. `0` significa sin límite (valor por defecto).

```js
xhr.timeout = 5000; // 5 segundos
xhr.ontimeout = () => console.error('La petición tardó demasiado');
```

## Propiedades de respuesta

| Propiedad | Descripción |
|---|---|
| `xhr.status` | Código HTTP (200, 404, 500…) |
| `xhr.statusText` | Texto del código (`"OK"`, `"Not Found"`) |
| `xhr.response` | Respuesta parseada según `responseType` |
| `xhr.responseText` | Respuesta como string (solo cuando `responseType` es `""` o `"text"`) |
| `xhr.responseURL` | URL final (tras redirecciones) |
| `xhr.getResponseHeader(n)` | Cabecera individual de la respuesta |
| `xhr.getAllResponseHeaders()` | Todas las cabeceras como string |

## `xhr.abort()`

Cancela la petición en curso. Dispara el evento `abort` y pone `readyState` a `UNSENT` (0).

```js
const xhr = new XMLHttpRequest();
xhr.open('GET', '/archivo-grande.zip');
xhr.send();

// Cancelar si el usuario hace clic en "Cancelar"
document.querySelector('#cancelar').addEventListener('click', () => xhr.abort());
```

## Recetas

### GET con JSON

```js
function getJSON(url) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.responseType = 'json';
    xhr.onload = () => {
      if (xhr.status >= 200 && xhr.status < 300) {
        resolve(xhr.response);
      } else {
        reject(new Error(`HTTP ${xhr.status}: ${xhr.statusText}`));
      }
    };
    xhr.onerror = () => reject(new Error('Error de red'));
    xhr.ontimeout = () => reject(new Error('Timeout'));
    xhr.timeout = 8000;
    xhr.send();
  });
}

// Uso
getJSON('/api/productos').then(datos => console.log(datos));
```

### POST con JSON

```js
function postJSON(url, payload) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open('POST', url);
    xhr.setRequestHeader('Content-Type', 'application/json');
    xhr.responseType = 'json';
    xhr.onload = () => {
      if (xhr.status >= 200 && xhr.status < 300) {
        resolve(xhr.response);
      } else {
        reject(new Error(`HTTP ${xhr.status}`));
      }
    };
    xhr.onerror = () => reject(new Error('Error de red'));
    xhr.send(JSON.stringify(payload));
  });
}

postJSON('/api/usuarios', { nombre: 'Ana', email: 'ana@example.com' })
  .then(res => console.log('Creado:', res));
```

## Cómo funciona por dentro

`open()` inicializa la máquina de estados de XHR: asocia el método y la URL, fija `readyState = OPENED` (1) y resetea el estado de respuesta. No abre ninguna conexión TCP todavía. `send()` serializa el body si es necesario, aplica las cabeceras acumuladas y despacha la petición HTTP a través de la red stack del navegador. A partir de ahí la máquina de estados avanza automáticamente (ver [[02 Eventos y readyState|Eventos y readyState]]).

> [!tip]
> Envolver XHR en una `Promise` (como en las recetas) permite consumirla con `async/await` y hace el código idiomáticamente equivalente a [[01 Fetch API/index|Fetch API]]. Para código nuevo, Fetch es la opción directa; la envoltura XHR/Promise tiene sentido solo cuando se necesita `upload.onprogress`.

> [!warning]
> No fijar `responseType = 'json'` y asumir que `xhr.response` ya es un objeto: en ese caso es un string y hay que llamar `JSON.parse()` explícitamente. Si `JSON.parse()` lanza, la petición sigue marcada como exitosa (`status 200`) aunque el body esté truncado o sea inválido.

## Notas relacionadas

- [[02 Eventos y readyState|Eventos y readyState]] — estados de XHR, eventos de ciclo de vida y progreso
- [[01 Fetch API/index|Fetch API]] — API moderna equivalente
- [[03 JSON/02 JSON.stringify (replacer, space)|JSON.stringify]] — serializar el body de una petición POST
- [[03 JSON/03 JSON.parse (reviver)|JSON.parse]] — deserializar la respuesta cuando no se usa `responseType = 'json'`
