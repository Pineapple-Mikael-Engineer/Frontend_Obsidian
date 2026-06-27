---
title: navigator.onLine — conectividad de red
aliases:
  - navigator.onLine
  - evento online
  - evento offline
  - Network Information API
  - navigator.connection
tags:
  - javascript
  - api/objeto
  - browser
draft: false
order: 2
---

# `navigator.onLine`

> [!definicion]
> `navigator.onLine` es un booleano que indica si el browser cree tener conexión de red. `false` significa definitivamente sin conexión; `true` puede significar conectado a internet o conectado solo a una red local. Los eventos `online` y `offline` en `window` se disparan cuando este estado cambia. Para detalles de calidad de red, la Network Information API (`navigator.connection`) ofrece información adicional aunque experimental.

```js
// Estado actual
if (!navigator.onLine) {
  mostrarBannerSinConexion();
}

// Escuchar cambios de conectividad
window.addEventListener("offline", () => {
  mostrarBannerSinConexion();
  deshabilitarFormularios();
});

window.addEventListener("online", () => {
  ocultarBannerSinConexion();
  sincronizarDatos();
});
```

## Propiedades y eventos de conectividad

| Propiedad / Evento | Tipo | Descripción |
|---|---|---|
| `navigator.onLine` | `boolean` | `true` si hay conexión de red detectada; `false` si no hay |
| `window` evento `"online"` | `Event` | Se dispara cuando el browser detecta conexión |
| `window` evento `"offline"` | `Event` | Se dispara cuando el browser pierde la conexión |
| `navigator.connection` | `NetworkInformation` | Network Information API (experimental, Chrome/Android) |
| `navigator.connection.effectiveType` | `"4g"` `"3g"` `"2g"` `"slow-2g"` | Tipo de conexión estimado |
| `navigator.connection.downlink` | `number` | Ancho de banda estimado en Mbps |
| `navigator.connection.rtt` | `number` | Round-trip time estimado en ms |
| `navigator.connection.saveData` | `boolean` | El usuario tiene Data Saver activado |

## Limitación importante de `navigator.onLine`

`navigator.onLine` detecta si el dispositivo está conectado a **alguna red**, no si hay acceso real a internet. Los casos donde da falsos positivos:

- El dispositivo está conectado a una red WiFi sin acceso a internet (router sin WAN).
- La conexión está establecida pero el servidor específico es inaccesible.
- Una VPN activa pero sin tráfico real.
- Redes cautivas (hoteles, aeropuertos) donde el usuario aún no se ha autenticado.

Por eso `false` es definitivo (no hay ni red), pero `true` es solo un indicativo optimista. Para verificación real hay que hacer una petición de red:

```js
async function verificarConexionReal() {
  if (!navigator.onLine) return false; // Fast path — definitivamente sin red

  try {
    // Petición a un endpoint pequeño y fiable
    const respuesta = await fetch("/ping", {
      method: "HEAD",
      cache: "no-store",
      signal: AbortSignal.timeout(5000),
    });
    return respuesta.ok;
  } catch {
    return false;
  }
}
```

## Receta: banner de "sin conexión"

```js
const banner = document.getElementById("banner-sin-conexion");

function actualizarEstadoRed() {
  banner.hidden = navigator.onLine;
  banner.textContent = "Sin conexión — los cambios se guardarán al recuperar la red";
}

// Estado inicial al cargar
actualizarEstadoRed();

// Actualizar cuando cambie
window.addEventListener("online", actualizarEstadoRed);
window.addEventListener("offline", actualizarEstadoRed);
```

## Receta: deshabilitar formularios cuando no hay conexión

```js
function sincronizarEstadoFormularios() {
  const formularios = document.querySelectorAll("form[data-requiere-red]");
  formularios.forEach((form) => {
    const botones = form.querySelectorAll("button[type='submit'], input[type='submit']");
    botones.forEach((btn) => {
      btn.disabled = !navigator.onLine;
      btn.title = navigator.onLine ? "" : "Sin conexión — no se puede enviar";
    });
  });
}

window.addEventListener("online", sincronizarEstadoFormularios);
window.addEventListener("offline", sincronizarEstadoFormularios);
sincronizarEstadoFormularios();
```

## Network Information API — `navigator.connection`

Disponible en Chrome y Android (no en Firefox ni Safari). Permite adaptar la experiencia según la calidad de la red:

```js
if ("connection" in navigator) {
  const conn = navigator.connection;

  console.log(conn.effectiveType); // "4g", "3g", "2g", "slow-2g"
  console.log(conn.downlink);      // 10.5 (Mbps estimados)
  console.log(conn.rtt);           // 50 (ms de round-trip estimado)
  console.log(conn.saveData);      // true si el usuario tiene Data Saver

  // Adaptar la calidad de las imágenes según la conexión
  if (conn.saveData || conn.effectiveType === "slow-2g") {
    cargarImagenesEnBajaCalidad();
  } else {
    cargarImagenesEnAltaCalidad();
  }

  // Escuchar cambios en la calidad de la conexión
  conn.addEventListener("change", () => {
    console.log("Tipo de conexión cambiado:", conn.effectiveType);
  });
}
```

## Cómo funciona por dentro

`navigator.onLine` es una propiedad expuesta por el proceso browser al renderer. El proceso browser monitoriza el estado de las interfaces de red del OS a través de las APIs del sistema operativo (`NetworkChange` en Windows, `SCNetworkReachability` en macOS, `netlink` en Linux). Cuando el OS reporta que todas las interfaces de red están desconectadas, el browser establece `onLine = false` y dispara el evento `offline` en todos los contextos activos.

Los eventos `online` y `offline` en `window` son sintéticos — no corresponden a eventos del DOM sino a notificaciones del proceso browser al renderer. Son confiables para el caso de desconexión completa del dispositivo (modo avión, cable desconectado) pero no para degradación de la calidad de la conexión.

La Network Information API (`connection`) estima `effectiveType` y `downlink` midiendo las respuestas de red reales de forma pasiva — no hace peticiones adicionales para medirlo. La estimación puede desfasarse si el usuario pasa de WiFi a datos móviles.

> [!tip]
> Para apps que deben funcionar offline, combinar `navigator.onLine` con un Service Worker es el patrón correcto: el SW intercepta las fetches y sirve desde la caché cuando `onLine` es `false`. El evento `online` es la señal para que el SW sincronice los datos pendientes con el servidor (background sync).

> [!warning]
> No confiar en `navigator.onLine` como única fuente de verdad para lógica crítica de negocio (ej. "¿puedo cobrar este pago?"). Siempre hay que manejar los errores de red en cada `fetch` individualmente — `navigator.onLine` puede ser `true` y la petición puede fallar igualmente por timeouts, CORS, errores del servidor, etc.

## Notas relacionadas

- [[index|Navigator]] — visión general del objeto navigator
- [[01 userAgent y language|userAgent y language]] — identificación del browser
- [[../../08 Comunicación con APIs/index|Comunicación con APIs]] — fetch, errores de red y manejo de peticiones fallidas
