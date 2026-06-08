---
title: Eventos de Medios
aliases:
  - HTMLMediaElement events
  - eventos de audio y video
  - media events
tags:
  - javascript
  - api/evento
  - eventos
draft: false
---

# Eventos de Medios

> [!definicion]
> Los eventos de medios se disparan en elementos `<audio>` y `<video>`, que implementan la interfaz `HTMLMediaElement`. Estos eventos notifican sobre el estado de reproducción, la carga de datos, cambios de volumen y errores. La interfaz del evento en sí es la base `Event`, pero las propiedades relevantes se leen directamente del elemento media (`e.target`) a través de `HTMLMediaElement`.

## Tabla de eventos

| Evento | Cuándo se dispara |
|---|---|
| loadedmetadata | El navegador conoce la duración, dimensiones y pistas del media |
| loadeddata | El frame actual tiene datos suficientes para reproducir |
| canplay | El navegador puede empezar a reproducir (puede que con pausas para buffering) |
| canplaythrough | El navegador puede reproducir el media completo sin pausas |
| play | Reproducción iniciada (incluyendo después de una pausa) |
| pause | Reproducción pausada |
| ended | La reproducción llegó al final |
| timeupdate | La posición de reproducción (currentTime) cambió |
| volumechange | El volumen o el estado mute cambiaron |
| waiting | Reproducción detenida esperando más datos (buffering) |
| error | Error al cargar o reproducir el media |
| seeking | El usuario está buscando una posición (scrubbing) |
| seeked | El seeking terminó y el media está en la nueva posición |
| ratechange | La velocidad de reproducción (playbackRate) cambió |

## Propiedades clave de HTMLMediaElement

Estas propiedades se leen en `e.target` (o en la referencia directa al elemento) dentro de los handlers:

```javascript
const video = document.getElementById('mi-video');

video.addEventListener('loadedmetadata', () => {
  console.log('Duración:       ', video.duration);        // segundos (número)
  console.log('Ancho nativo:   ', video.videoWidth);      // solo en <video>
  console.log('Alto nativo:    ', video.videoHeight);     // solo en <video>
  console.log('Pistas de audio:', video.audioTracks?.length);
});

video.addEventListener('timeupdate', () => {
  console.log('Tiempo actual:', video.currentTime);  // segundos (con decimales)
  console.log('Buffered:     ', video.buffered);     // TimeRanges — lo que está en caché
});

video.addEventListener('volumechange', () => {
  console.log('Volumen:', video.volume);  // 0.0 a 1.0
  console.log('Muted:  ', video.muted);  // boolean
});
```

Propiedades completas del `HTMLMediaElement`:
- `currentTime` — posición actual en segundos. Escribible: `video.currentTime = 60` hace seek.
- `duration` — duración total en segundos. `NaN` si no está disponible aún.
- `paused` — `true` si está pausado.
- `ended` — `true` si llegó al final.
- `volume` — de 0.0 a 1.0. Escribible.
- `muted` — boolean. Escribible.
- `playbackRate` — velocidad de reproducción. 1.0 es normal, 2.0 es el doble de rápido. Escribible.
- `readyState` — entero de 0 a 4 que indica cuántos datos están disponibles.
- `networkState` — estado de la conexión de red del media.
- `buffered` — objeto `TimeRanges` con los intervalos ya descargados.
- `src` — URL del media. Escribible para cambiar la fuente.
- `loop` — boolean. Escribible.

## timeupdate — frecuencia de disparo

`timeupdate` se dispara aproximadamente 4 veces por segundo mientras el media está reproduciendo (el estándar dice que el intervalo no puede ser menor a 250ms). No usar para sincronización precisa de efectos; para eso, usar `requestAnimationFrame` con `video.currentTime` en el loop.

```javascript
video.addEventListener('timeupdate', () => {
  const porcentaje = (video.currentTime / video.duration) * 100;
  barraProgreso.style.width = `${porcentaje}%`;
  tiempoActual.textContent = formatearTiempo(video.currentTime);
});

function formatearTiempo(segundos) {
  const mins = Math.floor(segundos / 60);
  const segs = Math.floor(segundos % 60).toString().padStart(2, '0');
  return `${mins}:${segs}`;
}
```

## loadedmetadata: el primer evento útil

Hasta que `loadedmetadata` se dispara, `video.duration` es `NaN` y `video.videoWidth/Height` son 0. Es el lugar correcto para configurar la UI de reproducción basada en los metadatos.

```javascript
video.addEventListener('loadedmetadata', () => {
  duracionTotal.textContent = formatearTiempo(video.duration);
  barraProgreso.max = video.duration;
  miniatura.width = video.videoWidth;
  miniatura.height = video.videoHeight;
});
```

## Control del elemento media

```javascript
// Los métodos de control retornan Promises (play puede rechazar si el autoplay está bloqueado)
async function reproducir() {
  try {
    await video.play();
  } catch (err) {
    if (err.name === 'NotAllowedError') {
      console.warn('Autoplay bloqueado por el navegador — requiere interacción del usuario');
    }
  }
}

function pausar() { video.pause(); }
function toggleMute() { video.muted = !video.muted; }
function cambiarVelocidad(rate) { video.playbackRate = rate; }
function irA(segundos) { video.currentTime = segundos; }
```

## Receta: barra de progreso custom sincronizada

```javascript
const video = document.getElementById('video-principal');
const barra = document.getElementById('barra-progreso');
const tiempoEl = document.getElementById('tiempo-actual');

// Actualizar barra mientras se reproduce
video.addEventListener('timeupdate', () => {
  if (!video.duration) return;
  barra.value = video.currentTime;
});

video.addEventListener('loadedmetadata', () => {
  barra.max = video.duration;
});

// Hacer seek cuando el usuario arrastra la barra
barra.addEventListener('input', () => {
  video.currentTime = barra.value;
});

// Mostrar tiempo buffereado
video.addEventListener('progress', () => {
  if (video.buffered.length > 0) {
    const bufferedEnd = video.buffered.end(video.buffered.length - 1);
    const porcentajeBuf = (bufferedEnd / video.duration) * 100;
    barraBuffer.style.width = `${porcentajeBuf}%`;
  }
});
```

## Receta: reanudar desde donde se dejó

```javascript
const STORAGE_KEY = 'video-progreso';

// Restaurar posición al cargar
video.addEventListener('loadedmetadata', () => {
  const guardado = parseFloat(localStorage.getItem(STORAGE_KEY));
  if (guardado && guardado < video.duration - 5) {
    video.currentTime = guardado;
  }
});

// Guardar posición cada vez que cambia
video.addEventListener('timeupdate', () => {
  // Guardar cada 5 segundos para no saturar localStorage
  if (Math.floor(video.currentTime) % 5 === 0) {
    localStorage.setItem(STORAGE_KEY, video.currentTime);
  }
});

// Limpiar al terminar
video.addEventListener('ended', () => {
  localStorage.removeItem(STORAGE_KEY);
});
```

## Cómo funciona por dentro

El modelo de carga del media tiene cuatro fases reflejadas en `readyState`:
- `HAVE_NOTHING (0)` — sin información.
- `HAVE_METADATA (1)` — `loadedmetadata` se disparó; duración y dimensiones disponibles.
- `HAVE_CURRENT_DATA (2)` — `loadeddata`; el frame actual es reproducible.
- `HAVE_FUTURE_DATA (3)` — `canplay`; hay datos suficientes para seguir reproduciendo.
- `HAVE_ENOUGH_DATA (4)` — `canplaythrough`; el navegador estima que puede reproducir sin interrupciones.

El evento `waiting` ocurre cuando `readyState` baja de `HAVE_FUTURE_DATA` durante la reproducción. `canplay` se vuelve a disparar cuando hay suficientes datos para continuar.

> [!tip]
> Para evitar el flash de controles nativos del navegador, usar `video.controls = false` y construir los controles en HTML/CSS. Los controles custom son más accesibles si se añaden los atributos `aria-*` correctos y se manejan eventos de teclado (Espacio para play/pause, flechas para seek, M para mute).

> [!warning]
> `video.play()` retorna una Promise que puede rechazar con `NotAllowedError` si el autoplay está bloqueado por la política del navegador. Siempre manejar la Promise con `try/await` o `.catch()`. Ignorar la Promise puede generar una excepción no capturada silenciosa en la consola.

## Notas relacionadas

- [[index]]
- [[04 Eventos de Ventana y Documento]]
- [[08 Eventos de Animación y Transición CSS]]
