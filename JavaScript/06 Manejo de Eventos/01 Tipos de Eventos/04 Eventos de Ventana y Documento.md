---
title: Eventos de Ventana y Documento
aliases:
  - window events
  - eventos del documento
  - eventos de ciclo de vida
tags:
  - javascript
  - api/evento
  - eventos
draft: false
---

# Eventos de Ventana y Documento

> [!definicion]
> Los eventos de ventana y documento notifican sobre el ciclo de vida de la página: cuándo el DOM está listo, cuándo todos los recursos se han cargado, cuándo el usuario está a punto de salir, y sobre cambios en el entorno de la ventana como el tamaño, el scroll o la visibilidad. La mayoría se escuchan en `window` o en `document`, no en elementos individuales.

## Tabla de eventos

| Evento | Objeto donde se escucha | Cuándo se dispara | Burbujea |
|---|---|---|---|
| DOMContentLoaded | document | El HTML está parseado y el DOM construido; scripts diferidos ejecutados | No |
| load | window | Todo está cargado: imágenes, CSS, iframes, scripts externos | No |
| beforeunload | window | Antes de que la página se descargue (navegación, cierre de pestaña) | No |
| unload | window | La página se está descargando (demasiado tarde para cancelar) | No |
| resize | window | El tamaño de la ventana del navegador cambia | No |
| scroll | window / elemento | El usuario hace scroll en la ventana o en un elemento con overflow | No |
| visibilitychange | document | La pestaña pasa a visible u oculta | No |
| online | window | El navegador recupera conexión a internet | No |
| offline | window | El navegador pierde conexión a internet | No |

## DOMContentLoaded vs load

La diferencia es el momento de disparo y qué recursos están disponibles:

```javascript
// DOMContentLoaded: el HTML está parseado, el DOM existe, se puede manipular.
// Los scripts <script defer> ya corrieron.
// Imágenes, CSS externos, iframes pueden NO estar cargados aún.
document.addEventListener('DOMContentLoaded', () => {
  // Seguro para: getElementById, querySelector, addEventListener en elementos
  // No seguro para: leer dimensiones de imágenes, fuentes web aplicadas
  inicializarApp();
});

// load: absolutamente todo está listo incluyendo recursos externos.
// Es el momento más tardío; esperar aquí retrasa la percepción de carga.
window.addEventListener('load', () => {
  // Seguro para: leer img.naturalWidth, leer dimensiones con fuentes web
  console.log('Todo cargado');
});
```

**Regla práctica:** casi siempre usar `DOMContentLoaded`. Reservar `load` para casos que genuinamente necesitan que las imágenes o fuentes estén disponibles (galería de imágenes, cálculos de layout con fuentes web).

Si el script está al final del `<body>` o usa `defer`, el DOM ya está listo cuando el script corre — `DOMContentLoaded` es innecesario en ese caso.

## resize

`resize` puede dispararse decenas de veces por segundo mientras el usuario arrastra el borde de la ventana. El handler debe ser barato computacionalmente.

```javascript
// MAL: recalcula layout en cada pixel de resize
window.addEventListener('resize', () => {
  recalcularLayoutComplejo(); // costoso
});

// BIEN: throttle con requestAnimationFrame — máximo una ejecución por frame
let rafId = null;
window.addEventListener('resize', () => {
  if (rafId) return;
  rafId = requestAnimationFrame(() => {
    recalcularLayoutComplejo();
    rafId = null;
  });
});
```

`window.innerWidth` y `window.innerHeight` dan el tamaño del viewport en el handler. Para el tamaño del viewport excluyendo las barras de herramientas: `document.documentElement.clientWidth/clientHeight`.

## scroll

`scroll` en `window` notifica el scroll del documento. `scroll` en un elemento específico notifica el scroll de ese elemento si tiene `overflow: scroll/auto`.

```javascript
// Detectar qué tan lejos se ha scrolleado (porcentaje)
window.addEventListener('scroll', () => {
  const scrollTop    = window.scrollY;
  const docHeight    = document.documentElement.scrollHeight - window.innerHeight;
  const porcentaje   = (scrollTop / docHeight) * 100;
  document.getElementById('barra-progreso').style.width = `${porcentaje}%`;
});

// Detectar si se llegó al fondo (infinite scroll)
window.addEventListener('scroll', () => {
  const distanciaAlFondo = document.documentElement.scrollHeight
    - window.scrollY
    - window.innerHeight;

  if (distanciaAlFondo < 200) {
    cargarMasContenido();
  }
});
```

> Para aplicaciones de alto rendimiento, reemplazar el listener de `scroll` con `IntersectionObserver`, que es más eficiente porque no requiere calcular posiciones en cada frame.

## visibilitychange

`document.visibilityState` puede ser `"visible"` u `"hidden"`. El evento `visibilitychange` se dispara en la transición entre ambos estados.

```javascript
document.addEventListener('visibilitychange', () => {
  if (document.visibilityState === 'hidden') {
    // La pestaña quedó en segundo plano
    pausarAnimaciones();
    pausarVideos();
    guardarEstadoTemporal();
  } else {
    // La pestaña volvió a ser visible
    reanudarAnimaciones();
  }
});
```

Más fiable que `blur`/`focus` de `window` porque detecta cuando el usuario cambia de pestaña, minimiza la ventana o bloquea la pantalla, no solo cuando cambia de aplicación.

## beforeunload

Permite mostrar un diálogo de confirmación antes de que el usuario salga. La especificación moderna requiere usar `e.preventDefault()` en lugar de devolver un string.

```javascript
let hayChangeSinGuardar = false;

window.addEventListener('beforeunload', (e) => {
  if (!hayChangeSinGuardar) return; // sin cambios: dejar salir sin diálogo

  // Forma estándar moderna (Chrome 119+, Firefox, Safari)
  e.preventDefault();

  // Forma legacy (todavía necesaria en Chrome < 119 y algunos otros)
  e.returnValue = '';
});
```

El texto del diálogo lo establece el navegador, no la aplicación — esta restricción es intencional para prevenir ingeniería social. No se puede personalizar el mensaje.

## online / offline

```javascript
window.addEventListener('offline', () => {
  mostrarBanner('Sin conexión — los cambios se guardarán localmente');
  activarModoOffline();
});

window.addEventListener('online', () => {
  ocultarBanner();
  sincronizarCambiosPendientes();
});

// Estado inicial (al cargar la página)
console.log('Conectado:', navigator.onLine);
```

## Receta: pausar video cuando la pestaña queda oculta

```javascript
const video = document.getElementById('video-principal');
let estabaReproduciendo = false;

document.addEventListener('visibilitychange', () => {
  if (document.visibilityState === 'hidden') {
    estabaReproduciendo = !video.paused;
    if (estabaReproduciendo) video.pause();
  } else {
    if (estabaReproduciendo) video.play();
  }
});
```

## Receta: guardar estado antes de salir

```javascript
window.addEventListener('beforeunload', (e) => {
  // Guardar en localStorage es sincrónico y funciona aquí
  localStorage.setItem('estado-app', JSON.stringify(obtenerEstadoActual()));

  // Si hay cambios sin sincronizar, advertir al usuario
  if (hayCambiosSinSincronizar()) {
    e.preventDefault();
    e.returnValue = '';
  }
});
```

## Cómo funciona por dentro

El ciclo de carga del documento sigue este orden: el navegador parsea el HTML y construye el DOM (`DOMContentLoaded`) → descarga y aplica CSS → descarga imágenes y otros recursos → dispara `load`. El evento `beforeunload` ocurre cuando el navegador está a punto de descargar el documento actual; en este punto aún se puede ejecutar código síncrono y escribir en `localStorage`, pero las peticiones `fetch` o `XMLHttpRequest` pueden no completarse. `unload` es demasiado tarde para la mayoría de operaciones.

> [!tip]
> Para persistir datos antes de que el usuario salga, `localStorage` o `sessionStorage` son más fiables que `fetch` en el handler de `beforeunload`/`unload`. Si necesitas enviar datos al servidor en ese momento, usar `navigator.sendBeacon()`, que garantiza el envío sin bloquear la descarga de la página.

> [!warning]
> Mostrar el diálogo de `beforeunload` innecesariamente es una mala práctica de UX y el navegador puede ignorarlo si la página no ha tenido interacción del usuario (política anti-abuso). Solo activarlo cuando haya cambios reales sin guardar.

## Notas relacionadas

- [[index]]
- [[03 Eventos de Formulario]]
- [[07 Eventos de Medios]]
