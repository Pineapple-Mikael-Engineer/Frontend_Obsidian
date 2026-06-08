---
title: FormData — construir y enviar datos de formulario
aliases:
  - FormData
tags:
  - javascript
  - api/clase
  - red
draft: false
---

# FormData

> [!definicion]
> `FormData` es una interfaz del navegador que representa un conjunto de pares clave/valor equivalentes a los campos de un formulario HTML, incluyendo archivos binarios. Se puede construir vacío o capturando automáticamente un `<form>` del DOM. El objeto resultante se pasa directamente como `body` a `fetch`, que lo codifica como `multipart/form-data` con el boundary correcto.

El caso central: capturar un formulario y enviarlo con Fetch sin serializar manualmente.

```js
const form = document.querySelector('#formulario-contacto');
const fd = new FormData(form);           // captura todos los campos con name
fd.append('origen', 'web');              // campo adicional

const res = await fetch('/api/contacto', { method: 'POST', body: fd });
```

## Bloques de esta sección

- [[01 Crear desde Formulario|Crear desde Formulario]] — constructor `new FormData()`, captura automática del `<form>`, iteradores `entries/keys/values`.
- [[02 Métodos (append, get, set)|Métodos (append, get, set)]] — API completa: `append`, `set`, `get`, `getAll`, `has`, `delete`, `forEach`.
- [[03 Enviar con Fetch|Enviar con Fetch]] — `body: fd`, boundary automático, comparativa multipart vs urlencoded vs JSON, progreso de subida.

## Cuándo usar FormData

| Escenario | Formato recomendado |
|---|---|
| Subir archivos (uno o varios) | FormData (multipart) |
| Campos de texto simples, sin archivos | URLSearchParams (urlencoded) o JSON |
| API REST con datos estructurados | JSON |
| Formulario HTML con inputs de tipo file | FormData |

## Notas relacionadas

- [[03 Enviar con Fetch|Enviar con Fetch]] — cómo pasar FormData como body sin romper el boundary
- [[../01 Fetch API/02 Configuración (method, headers, body)|Configuración de fetch]] — body, method y headers en detalle
- [[../03 JSON/index|JSON]] — alternativa para datos estructurados sin archivos
