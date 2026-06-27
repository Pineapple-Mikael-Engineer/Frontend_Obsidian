---
title: navigator.clipboard — portapapeles del sistema
aliases:
  - navigator.clipboard
  - Clipboard API
  - writeText
  - readText
  - ClipboardItem
  - copiar portapapeles
tags:
  - javascript
  - api/objeto
  - browser
draft: false
order: 5
---

# `navigator.clipboard`

> [!definicion]
> `navigator.clipboard` es la Clipboard API asíncrona para leer y escribir el portapapeles del sistema. Permite copiar texto, HTML e imágenes. **Escribir** (copiar) solo requiere que la llamada suceda en respuesta a un gesto del usuario. **Leer** (pegar) requiere permiso explícito `clipboard-read`. Disponible solo en HTTPS.

```js
// Copiar texto al portapapeles
await navigator.clipboard.writeText("Texto copiado");

// Leer texto del portapapeles (requiere permiso clipboard-read)
const texto = await navigator.clipboard.readText();
console.log(texto);

// Copiar contenido rico (imagen)
const blob = await fetch("/imagen.png").then(r => r.blob());
await navigator.clipboard.write([
  new ClipboardItem({ "image/png": blob })
]);
```

## Métodos de la API

| Método | Firma | Retorno | Requiere permiso |
|---|---|---|---|
| `writeText` | `(text: string)` | `Promise<void>` | Solo gesto del usuario |
| `readText` | `()` | `Promise<string>` | `clipboard-read` |
| `write` | `(items: ClipboardItem[])` | `Promise<void>` | Solo gesto del usuario |
| `read` | `()` | `Promise<ClipboardItem[]>` | `clipboard-read` |

`write` y `read` son para contenido rico (imágenes, HTML). Los tipos MIME soportados dependen del browser: `"text/plain"`, `"text/html"` e `"image/png"` están ampliamente soportados.

## `ClipboardItem`

`ClipboardItem` encapsula datos con su tipo MIME para copiar al portapapeles:

```js
// Copiar texto plano como ClipboardItem (equivalente a writeText)
const item = new ClipboardItem({
  "text/plain": new Blob(["Hola mundo"], { type: "text/plain" }),
});
await navigator.clipboard.write([item]);

// Copiar HTML enriquecido
const htmlBlob = new Blob(["<b>Texto en negrita</b>"], { type: "text/html" });
const textoBlob = new Blob(["Texto en negrita"], { type: "text/plain" });
await navigator.clipboard.write([
  new ClipboardItem({
    "text/html": htmlBlob,
    "text/plain": textoBlob, // Fallback para apps que no soportan HTML
  })
]);

// Leer items del portapapeles
const items = await navigator.clipboard.read();
for (const item of items) {
  if (item.types.includes("image/png")) {
    const blob = await item.getType("image/png");
    const url = URL.createObjectURL(blob);
    const img = document.createElement("img");
    img.src = url;
    document.body.appendChild(img);
  }
}
```

## Permisos

La API distingue entre escritura y lectura:

- **Escritura (`writeText`, `write`)**: Solo requiere que la llamada suceda dentro de un event handler de un gesto del usuario (`click`, `keydown`). No hay prompt de permiso. Si se llama fuera de un gesto (en un `setTimeout`, en un `fetch.then`), el browser puede rechazarla.
- **Lectura (`readText`, `read`)**: Requiere permiso `clipboard-read`. El browser muestra un prompt la primera vez. Se puede consultar el estado del permiso antes:

```js
const estado = await navigator.permissions.query({ name: "clipboard-read" });
// estado.state: "granted" | "denied" | "prompt"
```

## Fallback legacy: `document.execCommand("copy")`

Para browsers muy antiguos o contextos sin soporte de la Clipboard API:

```js
function copiarConFallback(texto) {
  // Intento moderno
  if (navigator.clipboard?.writeText) {
    return navigator.clipboard.writeText(texto);
  }

  // Fallback legacy (síncrono, deprecated)
  const el = document.createElement("textarea");
  el.value = texto;
  el.style.position = "fixed";    // Evitar scroll
  el.style.opacity = "0";
  document.body.appendChild(el);
  el.focus();
  el.select();

  try {
    const exito = document.execCommand("copy");
    if (!exito) throw new Error("execCommand copy falló");
  } finally {
    document.body.removeChild(el);
  }

  return Promise.resolve();
}
```

`execCommand("copy")` es síncrono y requiere que haya texto seleccionado en ese momento. Está deprecado en los estándares pero sigue funcionando en los browsers principales como fallback.

## Cómo funciona por dentro

El portapapeles es un recurso del OS compartido entre todas las aplicaciones. La Clipboard API actúa como intermediario entre JS y el portapapeles del sistema, con restricciones de seguridad importantes: el browser no puede dar acceso libre al portapapeles porque podría contener datos sensibles (contraseñas, tokens) del usuario.

Para `writeText`, el browser intercepta la llamada y, si ocurre en el contexto de un gesto del usuario, la escribe directamente en el portapapeles del OS a través de las APIs nativas (Win32 `SetClipboardData`, macOS `NSPasteboard`, X11 `XSetSelectionOwner`). Para `readText`, el permiso `clipboard-read` es necesario porque cualquier página podría leer silenciosamente el portapapeles del usuario sin que lo sepa.

`ClipboardItem` permite múltiples representaciones del mismo dato para diferentes apps: una copia puede tener tanto `text/html` (para apps que soporten rich text) como `text/plain` (como fallback), y el OS gestiona cuál servir según la app que pegue.

## Receta: botón "copiar al portapapeles" con feedback visual

```js
async function copiarEnlace(boton, texto) {
  try {
    await navigator.clipboard.writeText(texto);

    const textoOriginal = boton.textContent;
    boton.textContent = "¡Copiado!";
    boton.disabled = true;

    setTimeout(() => {
      boton.textContent = textoOriginal;
      boton.disabled = false;
    }, 2000);
  } catch (error) {
    console.error("No se pudo copiar:", error);
    boton.textContent = "Error al copiar";
  }
}

document.getElementById("btn-copiar").addEventListener("click", function () {
  copiarEnlace(this, window.location.href);
});
```

## Receta: pegar imagen desde el portapapeles

```js
document.addEventListener("paste", async (event) => {
  // Interceptar Ctrl+V / Cmd+V
  event.preventDefault();

  const items = await navigator.clipboard.read().catch(() => null);
  if (!items) return;

  for (const item of items) {
    if (!item.types.includes("image/png")) continue;

    const blob = await item.getType("image/png");
    const url = URL.createObjectURL(blob);

    const img = new Image();
    img.src = url;
    img.onload = () => URL.revokeObjectURL(url); // Liberar memoria

    document.getElementById("zona-imagen").appendChild(img);
  }
});
```

> [!tip]
> Para mostrar si la copia fue exitosa, usar el patrón "texto del botón cambia temporalmente": `"Copiar"` → `"¡Copiado!"` durante 2 segundos. Es más claro que un toast separado y no requiere UI adicional. Con CSS `transition` en el botón se puede animar suavemente el cambio de texto.

> [!warning]
> `navigator.clipboard` puede ser `undefined` en HTTP o en iframes sin el atributo `allow="clipboard-write"`. Siempre verificar con `navigator.clipboard?.writeText` (optional chaining) antes de llamar. Además, en Firefox la lectura del portapapeles (`readText`) fuera de un handler de `paste` event siempre requiere prompt de permiso, incluso con el permiso `clipboard-read` ya concedido.

## Notas relacionadas

- [[index|Navigator]] — visión general del objeto navigator
- [[06 share|share]] — compartir contenido con el OS (complementario al portapapeles)
- [[04 mediaDevices|mediaDevices]] — otra API de captura de datos del sistema
