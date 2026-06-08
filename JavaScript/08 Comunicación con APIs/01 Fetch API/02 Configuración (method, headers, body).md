---
title: fetch — configuración (method, headers, body)
aliases:
  - fetch opciones
  - fetch method headers body
  - RequestInit
tags:
  - javascript
  - api/funcion
  - red
objeto: global
tipo: funcion
retorna: Promise<Response>
muta: false
asincrono: true
draft: false
---

# Configuración de `fetch` — method, headers, body

> [!definicion]
> El segundo argumento de `fetch(url, opciones)` es un objeto `RequestInit` que configura la petición. Los tres campos más usados son `method` (verbo HTTP), `headers` (cabeceras) y `body` (cuerpo de la petición). Por defecto, `method` es `"GET"` y no hay body ni headers adicionales.

```js
const res = await fetch('/api/productos', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ nombre: 'Teclado', precio: 89 }),
});
```

## `method`

Cadena con el verbo HTTP. El navegador lo envía en mayúsculas por convención.

| Valor | Semántica | Permite body |
|---|---|---|
| `"GET"` | Leer un recurso (default) | No |
| `"POST"` | Crear un recurso | Sí |
| `"PUT"` | Reemplazar un recurso completo | Sí |
| `"PATCH"` | Modificar parcialmente un recurso | Sí |
| `"DELETE"` | Eliminar un recurso | Opcional |
| `"HEAD"` | Igual que GET pero sin body en la respuesta | No |

## `headers`

Objeto plano `{ clave: valor }` o instancia de `Headers`. Las cabeceras más comunes en peticiones:

| Cabecera | Cuándo ponerla | Ejemplo |
|---|---|---|
| `Content-Type` | Siempre que haya body (excepto FormData) | `application/json` |
| `Authorization` | APIs autenticadas con token | `Bearer eyJhbGc...` |
| `Accept` | Para indicar formato esperado de la respuesta | `application/json` |
| `X-Requested-With` | Para que el servidor detecte AJAX (legado) | `XMLHttpRequest` |

```js
// Objeto plano
headers: { 'Content-Type': 'application/json', 'Authorization': 'Bearer token123' }

// Instancia de Headers (programáticamente)
const h = new Headers();
h.set('Content-Type', 'application/json');
h.append('Accept', 'application/json');
headers: h
```

> [!warning]
> Con `FormData` como body **no poner `Content-Type` manualmente**. El navegador lo genera automáticamente con el `boundary` correcto (`multipart/form-data; boundary=----WebKitForm...`). Si se pone a mano, la petición falla porque el servidor no puede parsear el body sin el boundary correcto.

## `body`

El cuerpo de la petición. No válido en `GET` ni `HEAD`.

| Tipo de body | Content-Type resultante | Cuándo usar |
|---|---|---|
| `string` (JSON.stringify) | `application/json` (poner manualmente) | APIs REST con JSON |
| `FormData` | `multipart/form-data` + boundary (automático) | Formularios con archivos |
| `URLSearchParams` | `application/x-www-form-urlencoded` (automático) | Formularios simples sin archivos |
| `Blob` | El tipo del Blob (`image/png`, etc.) | Subir archivos binarios |
| `ArrayBuffer` | `application/octet-stream` (poner si necesario) | Datos binarios raw |
| `ReadableStream` | Según contenido | Subir un stream (chunked transfer) |

## Receta: POST JSON a una API REST

```js
async function crearProducto(datos) {
  const res = await fetch('/api/productos', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json',
    },
    body: JSON.stringify(datos),
  });

  if (!res.ok) {
    throw new Error(`Error ${res.status}: ${await res.text()}`);
  }

  return res.json(); // el producto creado con su id
}

const nuevo = await crearProducto({ nombre: 'Monitor', precio: 350 });
// → { id: 42, nombre: 'Monitor', precio: 350 }
```

## Receta: PUT con Authorization Bearer

```js
async function actualizarUsuario(id, cambios, token) {
  const res = await fetch(`/api/usuarios/${id}`, {
    method: 'PUT',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${token}`,
    },
    body: JSON.stringify(cambios),
  });

  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
}
```

## Receta: subir archivo con FormData

```js
async function subirImagen(inputFile) {
  const form = new FormData();
  form.append('imagen', inputFile.files[0]);
  form.append('descripcion', 'Avatar de usuario');

  // SIN Content-Type — el navegador lo gestiona
  const res = await fetch('/api/imagenes', { method: 'POST', body: form });
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
}
```

## Cómo funciona por dentro

El navegador serializa el objeto `RequestInit` y construye internamente un objeto `Request`. El `body` se convierte en un `ReadableStream` antes de enviarse. Para `JSON.stringify`, la serialización ocurre en el hilo JS antes de pasar el string al motor de red; para `FormData` y `Blob`, el motor de red gestiona la serialización directamente.

> [!tip]
> Para PATCH, incluir solo los campos que cambian. El servidor aplica los cambios parciales sobre el recurso existente. PUT reemplaza el recurso completo, por lo que omitir un campo equivale a borrarlo.

> [!warning]
> `JSON.stringify` lanza `TypeError` en valores circulares (`obj.self = obj`). También descarta propiedades con valor `undefined` y convierte `Date` a string ISO. Para estructuras complejas, verificar el resultado de `JSON.stringify` antes de enviarlo.

## Notas relacionadas

- [[01 Sintaxis Básica|Sintaxis Básica]] — firma y comportamiento base de fetch
- [[03 mode, credentials, cache|mode, credentials, cache]] — opciones adicionales de RequestInit
- [[04 Objeto Response|Objeto Response]] — qué devuelve la petición
- [[06 Manejo de Errores|Manejo de Errores]] — manejo de errores HTTP en la respuesta
