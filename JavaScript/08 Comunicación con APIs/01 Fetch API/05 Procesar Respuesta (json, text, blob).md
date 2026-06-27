---
title: Procesar Response — json, text, blob, arrayBuffer
aliases:
  - res.json
  - res.text
  - res.blob
  - res.arrayBuffer
  - procesar respuesta fetch
tags:
  - javascript
  - api/metodo
  - red
objeto: Response
tipo: metodo
retorna: Promise
muta: false
asincrono: true
draft: false
order: 5
---

# Procesar Respuesta — `json`, `text`, `blob`, `arrayBuffer`

> [!definicion]
> Los métodos de lectura del body en `Response` son async y retornan Promises. Consumen el `ReadableStream` del body y lo materializan en el tipo JavaScript correspondiente. Solo puede llamarse **uno** de ellos — el stream se consume al leer y `bodyUsed` pasa a `true`. Para leer varias veces, usar [[04 Objeto Response#Response.clone() — leer el body más de una vez|Response.clone()]].

```js
const res = await fetch('/api/usuarios');
const usuarios = await res.json(); // Promise<any>
```

## Métodos disponibles

| Método | Tipo devuelto | Cuándo usar | Error si… |
|---|---|---|---|
| `res.json()` | `Promise<any>` | Body es JSON válido (APIs REST) | Body no es JSON válido → `SyntaxError` |
| `res.text()` | `Promise<string>` | HTML, texto plano, CSV, XML, debug | Rara vez falla |
| `res.blob()` | `Promise<Blob>` | Imágenes, PDFs, archivos binarios | Rara vez falla |
| `res.arrayBuffer()` | `Promise<ArrayBuffer>` | Datos binarios raw, audio, WebAssembly | Rara vez falla |
| `res.formData()` | `Promise<FormData>` | Body multipart (relay de formularios) | Body no es multipart → error |

## `res.json()` — datos de una API REST

```js
async function obtenerProducto(id) {
  const res = await fetch(`/api/productos/${id}`);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
  // equivale a: return JSON.parse(await res.text())
}

const producto = await obtenerProducto(5);
// → { id: 5, nombre: 'Monitor', precio: 350 }
```

`res.json()` llama internamente a `JSON.parse` sobre el texto del body. Lanza `SyntaxError` si el servidor devuelve algo que no es JSON válido (HTML de error 500, respuesta vacía, etc.).

## `res.text()` — contenido de texto

```js
// Leer HTML o texto plano
const res = await fetch('/about.html');
const html = await res.text();
document.body.innerHTML = html;

// Útil para debug: ver qué devuelve realmente el servidor
const res = await fetch('/api/datos');
if (!res.ok) {
  const cuerpo = await res.text();
  console.error('Servidor respondió:', cuerpo.slice(0, 500));
}
```

## `res.blob()` — archivos e imágenes

```js
// Mostrar imagen descargada desde la API
async function mostrarImagen(url, imgElement) {
  const res = await fetch(url);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);

  const blob = await res.blob();
  const objectUrl = URL.createObjectURL(blob);
  imgElement.src = objectUrl;

  // Liberar la URL cuando ya no se necesite
  imgElement.onload = () => URL.revokeObjectURL(objectUrl);
}
```

```js
// Ofrecer un archivo como descarga al usuario
async function descargarArchivo(url, nombreArchivo) {
  const res = await fetch(url);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);

  const blob = await res.blob();
  const objectUrl = URL.createObjectURL(blob);

  const a = document.createElement('a');
  a.href = objectUrl;
  a.download = nombreArchivo;
  a.click();

  URL.revokeObjectURL(objectUrl);
}

await descargarArchivo('/api/reporte/2024', 'reporte-2024.pdf');
```

## `res.arrayBuffer()` — datos binarios raw

```js
// Decodificar audio para Web Audio API
const res = await fetch('/audio/cancion.mp3');
const buffer = await res.arrayBuffer();

const audioCtx = new AudioContext();
const audioBuffer = await audioCtx.decodeAudioData(buffer);

const source = audioCtx.createBufferSource();
source.buffer = audioBuffer;
source.connect(audioCtx.destination);
source.start();
```

## Streaming de respuesta grande con `res.body.getReader()`

Para cuerpos grandes (archivos, LLM tokens) se puede leer el `ReadableStream` en chunks sin cargar todo en memoria:

```js
async function* streamRespuesta(url) {
  const res = await fetch(url);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);

  const reader = res.body.getReader();
  const decoder = new TextDecoder();

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;
    yield decoder.decode(value, { stream: true });
  }
}

// Uso: mostrar chunks conforme llegan (streaming de LLM)
for await (const chunk of streamRespuesta('/api/chat/stream')) {
  process.stdout.write(chunk); // o actualizar el DOM progresivamente
}
```

## Cómo funciona por dentro

Todos los métodos del body consumen el `ReadableStream` subyacente acumulando los chunks en un buffer interno. `json()` es equivalente a `text()` seguido de `JSON.parse()`. `blob()` crea un `Blob` a partir del buffer binario con el `Content-Type` de la respuesta como tipo del Blob. `arrayBuffer()` retorna el buffer binario directamente sin crear un `Blob`.

> [!tip]
> Cuando el servidor puede devolver JSON en éxito y texto/HTML en error, la estrategia más robusta es comprobar `res.ok` primero, y solo si es `true` llamar a `res.json()`. Si es `false`, leer con `res.text()` para obtener el mensaje de error del servidor.

> [!warning]
> `URL.createObjectURL(blob)` crea una referencia en memoria que no se libera automáticamente. Siempre llamar `URL.revokeObjectURL(url)` cuando el objeto URL ya no sea necesario (en el evento `onload` de una imagen o tras la descarga). La acumulación de object URLs en SPAs de larga vida provoca fugas de memoria.

## Notas relacionadas

- [[04 Objeto Response|Objeto Response]] — `bodyUsed`, `body` (ReadableStream), `clone()`
- [[06 Manejo de Errores|Manejo de Errores]] — SyntaxError de json(), manejo de errores HTTP antes de leer el body
- [[../03 JSON/index|JSON]] — JSON.parse y JSON.stringify en el contexto de fetch
