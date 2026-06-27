---
title: FormData — enviar con fetch (multipart/form-data)
aliases:
  - fetch FormData
  - subir archivo fetch
  - multipart/form-data fetch
tags:
  - javascript
  - api/clase
  - red
objeto: FormData
tipo: clase
muta: false
asincrono: true
draft: false
order: 3
---

# Enviar FormData con Fetch

> [!definicion]
> Al pasar un objeto `FormData` como `body` de `fetch`, el navegador serializa las entradas en formato `multipart/form-data` y establece automáticamente la cabecera `Content-Type` con el `boundary` generado. No se debe establecer `Content-Type` manualmente: hacerlo omite el boundary y el servidor no puede parsear el cuerpo.

```js
const fd = new FormData(document.querySelector('#form-subida'));

const res = await fetch('/api/subir', {
  method: 'POST',
  body: fd,
  // ⚠ NO incluir Content-Type aquí — el browser lo pone solo
});

if (!res.ok) throw new Error(`HTTP ${res.status}`);
const resultado = await res.json();
```

La cabecera que el browser envía tiene esta forma:

```text
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryABC123xyz
```

## Comparativa de formatos de body

| Característica | FormData (multipart) | URLSearchParams (urlencoded) | JSON |
|---|---|---|---|
| Content-Type automático | `multipart/form-data` | `application/x-www-form-urlencoded` | Hay que ponerlo a mano |
| Soporta archivos binarios | Sí | No | No (solo base64, ineficiente) |
| Campos de texto | Sí | Sí | Sí |
| Valores múltiples por clave | Sí | Sí | Depende del servidor |
| Overhead de codificación | Alto (boundary + headers por parte) | Bajo | Bajo |
| Cuándo usarlo | Formularios con archivos | Formularios solo texto, auth tradicional | APIs REST con datos estructurados |

## Receta: subir un archivo

```js
async function subirArchivo(inputFile) {
  const archivo = inputFile.files[0];
  if (!archivo) return;

  const fd = new FormData();
  fd.append('archivo', archivo);
  fd.append('categoria', 'documentos');

  const res = await fetch('/api/archivos', {
    method: 'POST',
    body: fd,
  });

  if (!res.ok) throw new Error(`HTTP ${res.status}: ${res.statusText}`);
  return res.json(); // { id, url, nombre }
}
```

## Receta: subir múltiples archivos

```js
async function subirGaleria(inputMultiple) {
  const fd = new FormData();

  for (const file of inputMultiple.files) {
    fd.append('fotos', file);   // misma clave, entradas múltiples
  }
  fd.append('album', 'vacaciones-2024');

  const res = await fetch('/api/galeria', { method: 'POST', body: fd });
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json(); // { subidos: 5, urls: [...] }
}
```

## Alternativa: URLSearchParams para solo texto

Cuando no hay archivos y el servidor espera `application/x-www-form-urlencoded`:

```js
const params = new URLSearchParams();
params.append('usuario', 'ana');
params.append('plan', 'pro');

const res = await fetch('/api/suscripcion', {
  method: 'POST',
  body: params,   // Content-Type: application/x-www-form-urlencoded (automático)
});
```

`URLSearchParams` como body produce la misma codificación que un formulario HTML sin archivos. No admite `Blob` ni `File`.

## Progreso de subida

Fetch no expone progreso de upload. Para monitorear cuánto se ha enviado se requiere `XMLHttpRequest`:

```js
function subirConProgreso(fd, onProgress) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open('POST', '/api/archivos');

    xhr.upload.addEventListener('progress', (e) => {
      if (e.lengthComputable) {
        onProgress(Math.round((e.loaded / e.total) * 100)); // 0-100
      }
    });

    xhr.addEventListener('load', () => {
      if (xhr.status >= 200 && xhr.status < 300) {
        resolve(JSON.parse(xhr.responseText));
      } else {
        reject(new Error(`HTTP ${xhr.status}`));
      }
    });

    xhr.addEventListener('error', () => reject(new Error('Error de red')));
    xhr.send(fd); // acepta FormData directamente
  });
}

// Uso
await subirConProgreso(fd, (pct) => console.log(`${pct}%`));
```

La propuesta `fetch` con progreso (`ReadableStream` como body con `fetch` y `TransformStream`) existe en estado experimental pero no está disponible en todos los navegadores a fecha de 2025.

## Cómo funciona por dentro

`multipart/form-data` codifica cada campo como una "parte" separada por un boundary aleatorio. Cada parte lleva sus propias cabeceras (`Content-Disposition`, opcionalmente `Content-Type` para archivos) seguidas del valor. Los campos de texto se codifican en UTF-8; los archivos binarios se transmiten tal cual, sin codificación adicional. Por eso es la única opción viable para archivos: `urlencoded` no maneja binarios y JSON requiere base64, triplicando el tamaño.

> [!tip]
> Para APIs que esperan JSON, convertir primero a objeto plano y usar `JSON.stringify`. Para formularios mixtos (texto + archivo), FormData con `multipart` es la única opción práctica. Si el servidor es propio, diseñarlo para aceptar `multipart` desde el inicio si algún campo puede ser archivo.

> [!warning]
> Establecer `Content-Type: 'multipart/form-data'` manualmente en las `headers` de fetch **rompe la petición**: el boundary que el navegador genera internamente no se incluye en la cabecera manual, y el servidor no puede delimitar las partes. Dejar que el browser lo gestione omitiendo por completo la cabecera `Content-Type`.

## Notas relacionadas

- [[01 Crear desde Formulario|Crear desde Formulario]] — construir el FormData a partir de un `<form>`
- [[02 Métodos (append, get, set)|Métodos (append, get, set)]] — añadir o modificar campos antes de enviar
- [[../01 Fetch API/02 Configuración (method, headers, body)|Configuración de fetch]] — opciones completas del segundo argumento
- [[../02 XMLHttpRequest (legado)/index|XMLHttpRequest]] — alternativa para progreso de subida
