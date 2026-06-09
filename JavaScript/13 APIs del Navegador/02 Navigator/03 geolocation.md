---
title: navigator.geolocation — posición geográfica
aliases:
  - navigator.geolocation
  - geolocation API
  - getCurrentPosition
  - watchPosition
  - GPS javascript
tags:
  - javascript
  - api/objeto
  - browser
draft: false
---

# `navigator.geolocation`

> [!definicion]
> `navigator.geolocation` es la API del browser para obtener la posición geográfica del dispositivo. Usa GPS, WiFi, torres de celular o IP para triangular la posición. Requiere **permiso explícito del usuario** y **HTTPS** (o `localhost`). Proporciona tanto la posición única (`getCurrentPosition`) como el seguimiento continuo (`watchPosition`).

```js
// Verificar disponibilidad antes de usar
if (!("geolocation" in navigator)) {
  console.error("Geolocation no está disponible en este browser");
  return;
}

// Obtener posición una vez
navigator.geolocation.getCurrentPosition(
  (position) => {
    const { latitude, longitude, accuracy } = position.coords;
    console.log(`Lat: ${latitude}, Lon: ${longitude}, Precisión: ${accuracy}m`);
  },
  (error) => {
    console.error("Error de geolocalización:", error.message);
  },
  { enableHighAccuracy: true, timeout: 10000, maximumAge: 60000 }
);
```

## Métodos de la API

| Método | Firma | Retorno | Descripción |
|---|---|---|---|
| `getCurrentPosition` | `(onSuccess, onError?, opciones?)` | `void` | Obtiene la posición actual una sola vez |
| `watchPosition` | `(onSuccess, onError?, opciones?)` | `number` (watchId) | Posición continua — llama a `onSuccess` cada vez que la posición cambia |
| `clearWatch` | `(watchId: number)` | `void` | Detiene el seguimiento iniciado con `watchPosition` |

## El objeto `GeolocationPosition`

El callback de éxito recibe un objeto `GeolocationPosition`:

| Propiedad | Tipo | Descripción |
|---|---|---|
| `position.coords.latitude` | `number` | Latitud en grados decimales (−90 a 90) |
| `position.coords.longitude` | `number` | Longitud en grados decimales (−180 a 180) |
| `position.coords.accuracy` | `number` | Precisión del radio en metros |
| `position.coords.altitude` | `number \| null` | Altitud sobre el nivel del mar en metros |
| `position.coords.altitudeAccuracy` | `number \| null` | Precisión de la altitud en metros |
| `position.coords.heading` | `number \| null` | Dirección de movimiento (0–360°, 0 = Norte) |
| `position.coords.speed` | `number \| null` | Velocidad en m/s |
| `position.timestamp` | `number` | Marca de tiempo Unix en ms de cuándo se obtuvo la posición |

`altitude`, `heading` y `speed` son `null` si el dispositivo no tiene los sensores necesarios (GPS completo). `accuracy` siempre está disponible.

## El objeto `GeolocationPositionError`

| Propiedad | Valores | Descripción |
|---|---|---|
| `error.code` | `1` = PERMISSION_DENIED | El usuario denegó el permiso |
| `error.code` | `2` = POSITION_UNAVAILABLE | No se pudo obtener la posición (GPS off, sin señal) |
| `error.code` | `3` = TIMEOUT | Se superó el `timeout` configurado sin obtener posición |
| `error.message` | `string` | Mensaje descriptivo (no localizado, para debugging) |

## Opciones de `getCurrentPosition` y `watchPosition`

| Opción | Tipo | Default | Descripción |
|---|---|---|---|
| `enableHighAccuracy` | `boolean` | `false` | Usar GPS real (más preciso, más lento, más batería) |
| `timeout` | `number` | `Infinity` | Tiempo máximo en ms antes de llamar al callback de error |
| `maximumAge` | `number` | `0` | Usar posición en caché si tiene menos de `maximumAge` ms |

Con `maximumAge: 60000` el browser puede devolver una posición guardada de hace hasta 1 minuto sin hacer una nueva medición. Útil para apps que no necesitan precisión en tiempo real.

## Cómo funciona por dentro

Cuando se llama a `getCurrentPosition`, el browser primero comprueba si el origen ya tiene permiso de geolocalización (grant persistido). Si no, muestra el prompt de permiso del browser (no de la app — el usuario lo ve como una ventana del browser, no del sitio). Si el usuario deniega, el permiso se guarda como "denegado" para ese origen y futuros intentos irán directamente al callback de error con `PERMISSION_DENIED`.

Una vez con permiso, el browser consulta las fuentes de posición en este orden (según `enableHighAccuracy`): GPS del hardware → WiFi positioning (escaneando redes cercanas y comparando con bases de datos) → Cell ID (torres de celular) → GeoIP (como último fallback, muy impreciso). El `accuracy` refleja qué fuente se usó: GPS puede dar 5m, WiFi ~50m, GeoIP puede ser kilómetros.

`watchPosition` mantiene un handle interno que registra un listener con el sistema de posición del OS. El browser llama al callback cada vez que la posición cambia más de cierto umbral (definido internamente). En móviles esto puede consumir batería si `enableHighAccuracy: true` mantiene el GPS activo.

## Receta: obtener coordenadas envuelto en Promise

```js
function obtenerPosicion(opciones = {}) {
  return new Promise((resolve, reject) => {
    if (!("geolocation" in navigator)) {
      reject(new Error("Geolocation no soportada"));
      return;
    }

    navigator.geolocation.getCurrentPosition(resolve, reject, {
      enableHighAccuracy: false,
      timeout: 10000,
      maximumAge: 300000, // 5 minutos de caché
      ...opciones,
    });
  });
}

// Uso con async/await
try {
  const pos = await obtenerPosicion();
  const { latitude, longitude } = pos.coords;
  mostrarEnMapa(latitude, longitude);
} catch (error) {
  if (error.code === 1) {
    mostrarMensaje("Permiso de ubicación denegado");
  } else if (error.code === 3) {
    mostrarMensaje("Tiempo de espera agotado — intenta de nuevo");
  } else {
    mostrarMensaje("No se pudo obtener la ubicación");
  }
}
```

## Receta: mapa con actualización de posición en tiempo real

```js
let watchId = null;
let marcador = null; // Marcador en el mapa (ej. Leaflet, Google Maps)

function iniciarSeguimiento() {
  if (watchId !== null) return; // Ya está activo

  watchId = navigator.geolocation.watchPosition(
    (position) => {
      const { latitude, longitude, accuracy } = position.coords;

      if (!marcador) {
        // Crear el marcador en el mapa la primera vez
        marcador = crearMarcador(latitude, longitude);
        centrarMapa(latitude, longitude);
      } else {
        // Actualizar la posición del marcador
        marcador.setLatLng([latitude, longitude]);
      }

      // Mostrar el círculo de precisión
      actualizarCirculoPrecision(latitude, longitude, accuracy);
    },
    (error) => {
      console.error("Error de seguimiento:", error.message);
      detenerSeguimiento();
    },
    { enableHighAccuracy: true, timeout: 15000, maximumAge: 0 }
  );
}

function detenerSeguimiento() {
  if (watchId !== null) {
    navigator.geolocation.clearWatch(watchId);
    watchId = null;
  }
}

// Limpiar al salir de la página
window.addEventListener("beforeunload", detenerSeguimiento);
```

> [!tip]
> Para apps de mapas o delivery, usar `maximumAge: 0` y `enableHighAccuracy: true` con `watchPosition`. Para apps que solo necesitan la ciudad aproximada (ej. mostrar ofertas locales), `maximumAge: 300000` (5 min) y `enableHighAccuracy: false` dan una UX más rápida y gastan menos batería. El `timeout` en `watchPosition` aplica a cada lectura individual — si el GPS tarda más de `timeout` en una lectura concreta, se llama al error callback pero el watch continúa.

> [!warning]
> La Geolocation API **solo funciona en HTTPS** (y en `localhost` para desarrollo). En HTTP, `navigator.geolocation` existe pero `getCurrentPosition` llama inmediatamente al error callback sin mostrar el prompt de permiso. Además, si el usuario denegó el permiso previamente, la única forma de resetear el permiso es que el usuario lo haga manualmente en la configuración del browser. Usar `navigator.permissions.query({ name: "geolocation" })` para comprobar el estado del permiso antes de llamar y evitar llamadas que siempre fallarán.

## Notas relacionadas

- [[index|Navigator]] — visión general del objeto navigator
- [[../../07 Programación Asíncrona/index|Programación Asíncrona]] — Promises y async/await para envolver la API de callbacks
- [[04 mediaDevices|mediaDevices]] — otra API que requiere permiso del usuario (cámara y micrófono)
