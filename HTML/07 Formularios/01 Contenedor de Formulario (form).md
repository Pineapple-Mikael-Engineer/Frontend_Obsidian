---
title: <form> — Contenedor de formulario
aliases:
  - form element
tags:
  - html
  - api/elemento
  - formularios
elemento: form
categoria: flujo
rol_implicito: form
vacio: false
draft: false
---

# Contenedor de Formulario (form)

> [!definicion]
> `<form>` agrupa los controles de un formulario y define **cómo y adónde** se envían sus datos. Sus dos atributos centrales son `action` (la URL que recibe los datos) y `method` (cómo se transmiten: GET o POST).

```html
<form action="/registro" method="post">
  <label for="email">Correo</label>
  <input type="email" id="email" name="email" required />
  <button type="submit">Enviar</button>
</form>
```

## Atributos

| Atributo | Valores | Descripción |
|----------|---------|-------------|
| `action` | URL | Dónde se envían los datos al hacer submit |
| `method` | `get` · `post` · `dialog` | Cómo se transmiten |
| `enctype` | ver abajo | Codificación de los datos (relevante con archivos) |
| `novalidate` | (booleano) | Desactiva la validación nativa del navegador |
| `autocomplete` | `on` · `off` | Autocompletado de los campos |
| `target` | `_self` · `_blank`… | Dónde se carga la respuesta |

## method: GET vs. POST

La diferencia entre los dos métodos es fundamental y tiene implicaciones de seguridad y de uso:

| | `method="get"` | `method="post"` |
|--|----------------|-----------------|
| Dónde van los datos | En la URL (`?email=…&q=…`) | En el cuerpo de la petición |
| Visibles en la barra | Sí | No |
| Longitud | Limitada | Prácticamente ilimitada |
| Cacheable / marcable | Sí | No |
| Uso típico | Búsquedas, filtros (idempotente) | Login, registro, datos sensibles, cambios |

> [!warning] Nunca datos sensibles por GET
> Con `GET`, los datos quedan en la URL: visibles en la barra, en el historial, en los logs del servidor y en la cabecera `Referer`. **Jamás** envíes contraseñas ni datos sensibles por GET. Para cualquier cosa que cree, modifique o contenga información privada, usa `POST`.

## enctype: codificación de los datos

`enctype` solo importa con `method="post"` y determina cómo se empaquetan los datos:

| `enctype` | Cuándo |
|-----------|--------|
| `application/x-www-form-urlencoded` | Por defecto; campos de texto normales |
| `multipart/form-data` | **Obligatorio** para subir archivos (`<input type="file">`) |
| `text/plain` | Raro; depuración |

> [!warning] Subir archivos exige multipart
> Un formulario con [[03 Tipos de input/11 input file | `<input type="file">`]] **debe** llevar `enctype="multipart/form-data"` y `method="post"`. Sin ello, solo se envía el nombre del archivo, no su contenido.

## Envío sin recargar (JavaScript)

En aplicaciones modernas, el envío se suele interceptar con JavaScript para enviar por `fetch` sin recargar la página:

```html
<form id="f">…</form>
```

```js
document.getElementById("f").addEventListener("submit", async (e) => {
  e.preventDefault();                 // evita la recarga
  const datos = new FormData(e.target);
  await fetch("/registro", { method: "POST", body: datos });
});
```

`FormData` recoge automáticamente todos los controles con `name`. El detalle del manejo vive en el curso de JavaScript.

## El botón de envío

Un control `<button type="submit">` o `<input type="submit">` dentro del `<form>` dispara el envío. Pulsar Enter en un campo de texto también lo hace, lo que es un comportamiento esperado de accesibilidad.

## Buenas prácticas

> [!tip] Recomendaciones
> - `POST` para cualquier cosa que cambie estado o contenga datos privados; `GET` para búsquedas y filtros.
> - `enctype="multipart/form-data"` siempre que haya subida de archivos.
> - Da `name` a todos los controles que deban enviarse.
> - Aunque manejes el envío con JS, mantén `action`/`method` correctos como respaldo y para la semántica.

## Errores comunes

> [!warning] Trampas
> - **Datos sensibles por GET**: quedan expuestos en la URL.
> - **Subir archivos sin `multipart/form-data`**: no llega el contenido.
> - **Controles sin `name`**: no se envían, aunque tengan valor.
> - **Botón fuera del `<form>`** sin asociar: no dispara el envío (se puede asociar con `form="idDelForm"`).

## Notas relacionadas

- [[02 Campos de Entrada (input)/index]] — los controles que el formulario agrupa.
- [[06 Botones (button)]] — el botón que dispara el envío.
- [[09 Validación de Formularios/index]] — validar antes de enviar (`novalidate` desactiva la nativa).
