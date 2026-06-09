---
title: navigator.mediaDevices — cámara, micrófono y pantalla
aliases:
  - navigator.mediaDevices
  - getUserMedia
  - getDisplayMedia
  - MediaStream
  - MediaRecorder
tags:
  - javascript
  - api/objeto
  - browser
draft: false
---

# `navigator.mediaDevices`

> [!definicion]
> `navigator.mediaDevices` es el punto de entrada a las APIs de captura de media del browser. Permite acceder a la cámara (`getUserMedia`), compartir la pantalla (`getDisplayMedia`) y listar los dispositivos disponibles (`enumerateDevices`). Todas las operaciones son asíncronas, retornan `MediaStream`, y requieren **HTTPS** y **permiso explícito del usuario**.

```js
// Acceder a cámara y micrófono
const stream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true });

// Mostrar el stream en un <video>
const videoEl = document.getElementById("preview");
videoEl.srcObject = stream;
videoEl.play();

// Al terminar: apagar todos los tracks (apaga la luz de la cámara)
stream.getTracks().forEach(track => track.stop());
```

## Métodos de la API

| Método | Firma | Retorno | Descripción |
|---|---|---|---|
| `getUserMedia` | `(constraints)` | `Promise<MediaStream>` | Accede a cámara y/o micrófono del dispositivo |
| `getDisplayMedia` | `(opciones?)` | `Promise<MediaStream>` | Comparte pantalla, ventana o pestaña específica |
| `enumerateDevices` | `()` | `Promise<MediaDeviceInfo[]>` | Lista todos los dispositivos de media disponibles |
| `getSupportedConstraints` | `()` | `object` | Retorna las constraints que soporta el browser |

## Constraints de `getUserMedia`

```js
// Forma simple
{ video: true, audio: true }

// Con opciones detalladas
{
  video: {
    width: { ideal: 1280 },
    height: { ideal: 720 },
    facingMode: "user",          // "user" (frontal) | "environment" (trasera)
    frameRate: { max: 30 },
  },
  audio: {
    echoCancellation: true,
    noiseSuppression: true,
    sampleRate: 44100,
  }
}

// Solo audio (para grabación de voz)
{ audio: true, video: false }

// Solo video (sin micrófono)
{ video: true, audio: false }
```

Las constraints son "ideales" — el browser intenta satisfacerlas pero puede devolver un stream aunque no se cumplan exactamente. Para constraints obligatorias se usa `exact`: `{ width: { exact: 1920 } }`.

## El objeto `MediaStream`

Un `MediaStream` es un flujo de media compuesto por uno o más `MediaStreamTrack`:

| Propiedad / Método | Descripción |
|---|---|
| `stream.getTracks()` | Array de todos los tracks (video + audio) |
| `stream.getVideoTracks()` | Solo los tracks de video |
| `stream.getAudioTracks()` | Solo los tracks de audio |
| `stream.id` | Identificador único del stream |
| `track.stop()` | Detiene el track y apaga el dispositivo (luz de cámara off) |
| `track.enabled` | `false` para pausar sin apagar (silenciar micrófono) |
| `track.label` | Nombre del dispositivo (requiere permiso previo) |
| `track.kind` | `"audio"` o `"video"` |
| `track.readyState` | `"live"` o `"ended"` |

La diferencia entre `track.stop()` y `track.enabled = false`: `stop()` apaga el dispositivo físico (la cámara se desactiva); `enabled = false` envía silencio/negro pero el dispositivo sigue encendido. Para "mutear" el micrófono sin cortar el stream: `audioTrack.enabled = false`.

## `enumerateDevices`

```js
// Sin permisos: solo deviceId y kind, label está vacío
// Con permisos: también label con el nombre real del dispositivo
const dispositivos = await navigator.mediaDevices.enumerateDevices();

const camaras = dispositivos.filter(d => d.kind === "videoinput");
const microfonos = dispositivos.filter(d => d.kind === "audioinput");
const altavoces = dispositivos.filter(d => d.kind === "audiooutput");

console.log(camaras.map(c => c.label));
// ["FaceTime HD Camera", "OBS Virtual Camera"] (con permiso)
// ["", ""] (sin permiso previo — label vacío)
```

## Cómo funciona por dentro

`getUserMedia` es una llamada asíncrona porque implica:
1. Mostrar el prompt de permiso del browser (si aún no hay grant).
2. Negociar con el proceso browser cuál dispositivo usar.
3. Inicializar el hardware (abrir el driver de cámara/micrófono).
4. Devolver el `MediaStream` con los tracks activos.

El `MediaStream` fluye desde el hardware hacia el JS en tiempo real. Cuando se asigna `videoEl.srcObject = stream`, el browser conecta el stream directamente al pipeline de renderizado del `<video>` — los frames de la cámara no pasan por JS (sería demasiado costoso), sino que van de hardware al compositor gráfico, con JS solo manejando el objeto stream como referencia.

La luz indicadora de la cámara está controlada por el driver del OS. Se apaga solo cuando todos los tracks de video están en estado `"ended"` (después de `track.stop()`). Simplemente pausar el video (`videoEl.pause()`) no apaga la cámara.

## Receta: acceder a la cámara y mostrar el feed en un `<video>`

```js
async function iniciarCamara() {
  const videoEl = document.getElementById("camara");

  try {
    const stream = await navigator.mediaDevices.getUserMedia({
      video: { facingMode: "user", width: { ideal: 1280 } },
      audio: false,
    });

    videoEl.srcObject = stream;
    videoEl.play();

    // Guardar referencia al stream para poder apagarlo después
    return stream;
  } catch (error) {
    if (error.name === "NotAllowedError") {
      console.error("El usuario denegó el acceso a la cámara");
    } else if (error.name === "NotFoundError") {
      console.error("No se encontró ninguna cámara en este dispositivo");
    } else if (error.name === "NotReadableError") {
      console.error("La cámara está siendo usada por otra aplicación");
    } else {
      throw error;
    }
  }
}

function detenerCamara(stream) {
  stream?.getTracks().forEach(track => track.stop());
}
```

## Receta: grabar audio con `MediaRecorder`

```js
async function grabarAudio(duracionMs = 5000) {
  const stream = await navigator.mediaDevices.getUserMedia({ audio: true, video: false });
  const chunks = [];

  const recorder = new MediaRecorder(stream, { mimeType: "audio/webm;codecs=opus" });

  recorder.addEventListener("dataavailable", (e) => {
    if (e.data.size > 0) chunks.push(e.data);
  });

  recorder.addEventListener("stop", () => {
    const blob = new Blob(chunks, { type: "audio/webm" });
    const url = URL.createObjectURL(blob);

    // Reproducir o descargar
    const audio = new Audio(url);
    audio.play();

    // Limpiar
    stream.getTracks().forEach(t => t.stop());
  });

  recorder.start();

  // Detener después de duracionMs
  setTimeout(() => recorder.stop(), duracionMs);
}
```

> [!tip]
> Para capturar la pantalla completa (grabación de pantalla, screenshare en videollamadas), usar `getDisplayMedia` en lugar de `getUserMedia`. `getDisplayMedia` siempre muestra al usuario un selector de qué compartir (pestaña, ventana o pantalla completa) — no puede omitirse por diseño de seguridad. El stream resultante se usa igual que el de `getUserMedia`: se puede asignar a `<video>.srcObject` o a un `MediaRecorder`.

> [!warning]
> Siempre llama a `track.stop()` cuando el usuario termine de usar la cámara/micrófono. Si no se hace, la luz indicadora de la cámara permanece encendida y el dispositivo sigue ocupado, incluso si el usuario navega a otra página. Un buen lugar para limpiar es el evento `beforeunload` de `window` o el `disconnectedCallback` de un web component.

## Notas relacionadas

- [[index|Navigator]] — visión general del objeto navigator
- [[03 geolocation|geolocation]] — otra API que requiere permiso del usuario
- [[05 clipboard|clipboard]] — leer y escribir el portapapeles (también requiere permiso)
