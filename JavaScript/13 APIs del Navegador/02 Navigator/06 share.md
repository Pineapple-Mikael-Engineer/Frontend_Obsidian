---
title: navigator.share — Web Share API
aliases:
  - navigator.share
  - Web Share API
  - navigator.canShare
  - compartir web
tags:
  - javascript
  - api/objeto
  - browser
draft: false
---

# `navigator.share`

> [!definicion]
> `navigator.share(data)` activa el menú de compartir nativo del OS — el mismo que aparece cuando el usuario toca el botón de compartir en una app móvil. Permite compartir texto, URLs y archivos con cualquier app del sistema (WhatsApp, Telegram, Notas, Mail, etc.) sin que la web tenga que implementar integraciones individuales. Disponible principalmente en móviles y Safari macOS. Solo en HTTPS.

```js
// Compartir una URL con título
await navigator.share({
  title: "Artículo interesante",
  text: "Mira este artículo que encontré",
  url: "https://ejemplo.com/articulo",
});

// Comprobar disponibilidad antes de usar
if (navigator.share) {
  await navigator.share({ url: window.location.href });
} else {
  // Fallback: copiar al portapapeles
  await navigator.clipboard.writeText(window.location.href);
}
```

## Propiedades de `data`

| Propiedad | Tipo | Descripción |
|---|---|---|
| `title` | `string?` | Título del contenido compartido (el OS puede ignorarlo) |
| `text` | `string?` | Texto descriptivo |
| `url` | `string?` | URL a compartir |
| `files` | `File[]?` | Array de archivos (imágenes, documentos, etc.) |

Al menos una de las propiedades debe estar presente. `url` puede ser relativa — el browser la resuelve a absoluta. `title` no siempre se usa: WhatsApp lo ignora, por ejemplo.

## `navigator.canShare(data)`

Antes de compartir archivos, comprobar si el tipo de archivo es compatible:

```js
const archivo = new File(["contenido"], "nota.txt", { type: "text/plain" });

if (navigator.canShare({ files: [archivo] })) {
  await navigator.share({ files: [archivo], title: "Mi nota" });
} else {
  console.log("Este browser o dispositivo no soporta compartir archivos");
}
```

`canShare` también sirve para comprobar soporte general antes de llamar a `share`:

| Comprobación | Código |
|---|---|
| API disponible | `typeof navigator.share === "function"` |
| Data válida para share | `navigator.canShare({ url: "..." })` |
| Archivos compatibles | `navigator.canShare({ files: [file] })` |

## Disponibilidad por plataforma

| Plataforma | Disponibilidad |
|---|---|
| Android Chrome / Samsung Internet | Amplia — menú de compartir del OS |
| iOS Safari | Amplia — Share Sheet nativa de iOS |
| macOS Safari 12.1+ | Disponible |
| Chrome en escritorio (Windows/Linux/Mac) | Limitado — solo en algunos contextos desde Chrome 89 |
| Firefox escritorio | No soportado |
| Firefox Android | Disponible |

La estrategia correcta es siempre feature-detect con `if (navigator.share)` y proveer fallback.

## Cómo funciona por dentro

`navigator.share` delega completamente al OS la selección del destino de compartir. El browser empaqueta el `data` en el formato que entiende cada plataforma (Android `Intent.ACTION_SEND`, iOS `UIActivityViewController`) y lanza el picker nativo. El JS no tiene acceso a qué app eligió el usuario ni a si finalmente compartió el contenido — solo se sabe si el usuario canceló (la Promise se rechaza) o si el share fue entregado al OS (la Promise se resuelve).

La Promise se rechaza con `AbortError` si el usuario cierra el menú sin seleccionar nada. Esto es un rechazo normal de flujo — no es un error de programación.

La restricción de solo HTTPS y gesto del usuario existe porque `share` puede enviar datos a apps externas del dispositivo — una página podría filtrar datos sensibles de forma invisible si no hubiera estas restricciones.

## Receta: botón "compartir" con fallback a copiar enlace

```js
async function compartir(url, titulo) {
  if (navigator.share) {
    try {
      await navigator.share({ title: titulo, url });
      // Share entregado al OS (el usuario puede o no haber enviado — no sabemos)
    } catch (error) {
      if (error.name === "AbortError") {
        // El usuario canceló el menú — comportamiento normal, no mostrar error
        return;
      }
      // Error real — caer al fallback
      await fallbackCopiar(url);
    }
  } else {
    await fallbackCopiar(url);
  }
}

async function fallbackCopiar(url) {
  await navigator.clipboard.writeText(url);
  mostrarToast("Enlace copiado al portapapeles");
}

document.getElementById("btn-compartir").addEventListener("click", () => {
  compartir(window.location.href, document.title);
});
```

## Receta: compartir archivos (imágenes)

```js
async function compartirImagen(urlImagen) {
  // Descargar la imagen como Blob
  const respuesta = await fetch(urlImagen);
  const blob = await respuesta.blob();
  const archivo = new File([blob], "imagen.jpg", { type: blob.type });

  if (!navigator.canShare?.({ files: [archivo] })) {
    console.log("Compartir archivos no soportado — descargando en su lugar");
    descargarBlob(blob, "imagen.jpg");
    return;
  }

  try {
    await navigator.share({
      files: [archivo],
      title: "Mira esta imagen",
    });
  } catch (error) {
    if (error.name !== "AbortError") throw error;
  }
}
```

## Cómo funciona por dentro

La Web Share API fue diseñada con la filosofía de "el browser como intermediario seguro": la web nunca sabe a qué app compartió el usuario, y el OS es el árbitro de qué apps pueden recibir el share (el user-agent intenta filtrar las apps compatibles con el tipo de contenido). Esto es diferente a SDKs de compartir tipo "share to Twitter / WhatsApp" que abren la app directamente con deep links — esos links son específicos de cada app y requieren mantener integraciones por separado. La Web Share API es más genérica y accesible para el usuario.

> [!tip]
> Para una UX completa: mostrar el botón "Compartir" solo en plataformas donde `navigator.share` existe, y en su lugar mostrar "Copiar enlace" en escritorio. Esto evita que el fallback se sienta como una degradación — el usuario de escritorio espera copiar el enlace, no un share sheet.

> [!warning]
> `navigator.share` **solo puede llamarse desde un event handler** de un gesto explícito del usuario (click, tap, keydown). No funciona en `setTimeout`, en `.then()` de una Promise cuya resolución no está sincronizada con el gesto original, ni al cargar la página. Si se necesita hacer trabajo asíncrono antes de compartir (como fetch de una imagen), hacerlo antes del click y guardar el resultado, luego llamar a `share` directamente en el click handler.

## Notas relacionadas

- [[index|Navigator]] — visión general del objeto navigator
- [[05 clipboard|clipboard]] — fallback natural cuando share no está disponible
- [[01 userAgent y language|userAgent y language]] — detectar la plataforma para decidir mostrar share o clipboard
