---
title: Navigator — el agente de usuario
aliases:
  - Navigator
  - navigator
  - objeto navigator
tags:
  - javascript
  - api/objeto
  - browser
draft: false
order: 2
---

# `navigator`

> [!definicion]
> **`navigator`** es el objeto que representa al agente de usuario (el browser). Es una propiedad de `window` (`window.navigator`). Proporciona información sobre el entorno de ejecución (idioma, conectividad, plataforma, capacidades del dispositivo) y da acceso a APIs del sistema operativo como geolocalización, cámara, micrófono, portapapeles y el menú de compartir nativo.

```js
// Información básica del entorno
navigator.language;        // "es-ES"
navigator.onLine;          // true / false
navigator.userAgent;       // string largo — no usar para feature detection

// Comprobar si una API está disponible antes de usarla
if ("geolocation" in navigator) {
  navigator.geolocation.getCurrentPosition(onSuccess, onError);
}

if ("clipboard" in navigator) {
  await navigator.clipboard.writeText("Texto copiado");
}
```

El patrón `"api" in navigator` es la forma correcta de feature detection — mucho más fiable que inspeccionar `navigator.userAgent`.

## Bloques de esta sección

- [[01 userAgent y language|userAgent y language]] — identificación del browser, idioma preferido del usuario y Client Hints API.
- [[02 onLine|onLine]] — detectar conectividad de red y los eventos `online`/`offline`.
- [[03 geolocation|geolocation]] — obtener la posición geográfica del dispositivo con `getCurrentPosition` y `watchPosition`.
- [[04 mediaDevices|mediaDevices]] — acceso a cámara, micrófono y captura de pantalla con `getUserMedia`.
- [[05 clipboard|clipboard]] — leer y escribir el portapapeles del sistema de forma asíncrona.
- [[06 share|share]] — activar el menú de compartir nativo del OS con la Web Share API.

## Propiedades y APIs del objeto navigator

| Propiedad / API | Descripción | Requiere permiso |
|---|---|---|
| `navigator.userAgent` | String UA del browser (no fiable para detection) | No |
| `navigator.userAgentData` | Client Hints API — datos estructurados del browser | No (básico) / Sí (entropía alta) |
| `navigator.language` | Idioma preferido del usuario (`"es-ES"`) | No |
| `navigator.languages` | Array de idiomas en orden de preferencia | No |
| `navigator.onLine` | Booleano de conectividad de red | No |
| `navigator.connection` | Network Information API — velocidad y tipo de red | No |
| `navigator.geolocation` | API de posición geográfica | Sí |
| `navigator.mediaDevices` | Cámara, micrófono y captura de pantalla | Sí |
| `navigator.clipboard` | Portapapeles del sistema (lectura requiere permiso) | Sí (lectura) |
| `navigator.share` | Web Share API — menú de compartir del OS | No (pero solo móviles/Safari) |
| `navigator.permissions` | Consultar el estado de permisos | No |
| `navigator.serviceWorker` | Registro y gestión de Service Workers | No |
| `navigator.storage` | Cuota y persistencia de almacenamiento | No |
| `navigator.sendBeacon` | Enviar datos analíticos al servidor de forma fiable en `unload` | No |
| `navigator.vibrate` | Vibración del dispositivo (móviles) | No |
| `navigator.hardwareConcurrency` | Número de núcleos lógicos del CPU | No |

## Cómo funciona por dentro

`navigator` es un objeto `Navigator` (instancia de la interfaz Navigator de la plataforma web). Es un singleton por contexto de navegación — siempre es el mismo objeto en toda la vida del script. Sus propiedades son expuestas por el proceso browser al proceso renderer mediante IPC; las de acceso frecuente (como `language` y `onLine`) se leen desde un caché en el proceso renderer, y las que requieren IPC real (como las APIs con permiso) vuelven como promesas.

La mayoría de las APIs expuestas en `navigator` requieren HTTPS (`secure context`) para funcionar. Esto incluye `geolocation`, `mediaDevices`, `clipboard`, `share` y `serviceWorker`. En HTTP solo están disponibles en `localhost`.

> [!tip]
> Para comprobar si el contexto es seguro (HTTPS o localhost) antes de usar APIs restringidas: `window.isSecureContext` devuelve `true` si el contexto es seguro. Útil para mostrar un mensaje de error descriptivo en lugar de dejar que la API falle silenciosamente.

> [!warning]
> Muchas propiedades de `navigator` son de solo lectura y no son configurables. No se puede ni se debe modificar `navigator.language` o `navigator.userAgent` desde JS de la app — los cambios no persisten y pueden fallar silenciosamente. Para tests, usar mocks de `navigator` a nivel de test runner.

## Notas relacionadas

- [[../01 Window/index|Window]] — el objeto global del browser que contiene `navigator`
- [[../03 Location y URL/index|Location y URL]] — otra API del browser para gestionar la URL actual
- [[../../09 Almacenamiento en Cliente/index|Almacenamiento en Cliente]] — localStorage, sessionStorage, IndexedDB y Cookies
