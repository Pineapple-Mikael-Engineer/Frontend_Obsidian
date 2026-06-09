---
title: navigator.userAgent y navigator.language
aliases:
  - navigator.userAgent
  - navigator.language
  - navigator.languages
  - navigator.userAgentData
  - UA sniffing
  - Client Hints
tags:
  - javascript
  - api/objeto
  - browser
draft: false
---

# `userAgent` y `language`

> [!definicion]
> `navigator.userAgent` es el string de identificación del browser enviado en cada petición HTTP y expuesto en JS. **No es fiable** para detectar capacidades: los browsers lo falsifican por compatibilidad histórica. `navigator.language` y `navigator.languages` exponen el idioma preferido del usuario tal como lo ha configurado en el browser, y son la señal correcta para internacionalización en el cliente.

```js
// userAgent — string largo y poco fiable
navigator.userAgent;
// "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0 Safari/537.36"

// Idioma preferido del usuario
navigator.language;     // "es-ES"
navigator.languages;    // ["es-ES", "es", "en-US", "en"]

// Feature detection — la forma CORRECTA de detectar capacidades
if ("geolocation" in navigator) { /* soportado */ }
if (typeof fetch !== "undefined") { /* fetch disponible */ }
```

## Propiedades de identificación y locale

| Propiedad | Qué devuelve | Fiabilidad |
|---|---|---|
| `navigator.userAgent` | String UA completo del browser | Baja — los browsers falsifican el UA |
| `navigator.userAgentData` | Objeto estructurado de la Client Hints API | Alta — datos más limpios y verificables |
| `navigator.language` | Idioma preferido del usuario (primer idioma configurado) | Alta |
| `navigator.languages` | Array de idiomas en orden de preferencia | Alta |
| `navigator.platform` | Plataforma del OS (deprecado) | Baja — devuelve valores inconsistentes |
| `navigator.vendor` | Fabricante del browser | Media — solo diferencia grandes motores |

## Por qué `userAgent` no es fiable

El `userAgent` de Chrome 124 en Linux empieza por `Mozilla/5.0` — una herencia de 1994 cuando los servidores solo servían contenido rico a Netscape (Mozilla). Todos los browsers subsiguientes copiaron ese prefijo para no ser excluidos. Hoy el UA contiene capas de compatibilidad acumuladas durante 30 años. Chrome en Android incluye "Safari" en el UA. Opera incluye "Chrome". Edge incluye tanto "Chrome" como "Safari".

```js
// NUNCA hacer esto para detectar Chrome:
const esChrome = navigator.userAgent.includes("Chrome"); // También es true en Edge, Opera, Brave

// BIEN: detectar si la API concreta existe
const tieneWebBluetooth = "bluetooth" in navigator;
const tieneNotificaciones = "Notification" in window;
const tieneServiceWorker = "serviceWorker" in navigator;
```

## Client Hints API — `navigator.userAgentData`

La Client Hints API es el reemplazo moderno de `navigator.userAgent`. Disponible en Chrome/Edge 90+, no en Firefox/Safari (que prefieren no exponer estos datos).

```js
// Datos básicos — sincrónicos, baja entropía
if (navigator.userAgentData) {
  navigator.userAgentData.brands;
  // [{ brand: "Chromium", version: "124" }, { brand: "Google Chrome", version: "124" }, ...]

  navigator.userAgentData.mobile; // true en móviles
  navigator.userAgentData.platform; // "Linux", "Windows", "macOS", "Android", "iOS"
}

// Datos de alta entropía — asíncronos, requieren lista explícita
const datosCompletos = await navigator.userAgentData.getHighEntropyValues([
  "architecture",     // "x86", "arm"
  "bitness",          // "64"
  "platformVersion",  // "14.0.0" (macOS Sonoma)
  "uaFullVersion",    // "124.0.6367.62"
  "model",            // Modelo del dispositivo (móviles)
]);
console.log(datosCompletos.architecture); // "x86"
```

La distinción entre datos de baja y alta entropía es deliberada: los datos de alta entropía pueden usarse para fingerprinting, por eso requieren una llamada asíncrona explícita (y en el futuro podrán requerir permiso del usuario).

## `navigator.language` y `navigator.languages`

`language` es siempre el primer elemento de `languages`. El array refleja las preferencias del usuario en el orden en que las tiene configuradas en el browser.

```js
navigator.language;     // "es-ES"
navigator.languages;    // ["es-ES", "es", "en-US", "en"] (readonly, frozen array)
```

Los valores siguen el formato BCP 47: código de idioma + subtag de región (`"es-ES"`, `"en-US"`, `"zh-Hant-TW"`). Pueden ser solo idioma (`"es"`) si el usuario no tiene región configurada.

## Receta: detectar el locale del usuario para i18n

```js
function detectarLocale(localesDisponibles = ["es", "en"]) {
  // navigator.languages es el array completo de preferencias
  const preferencias = navigator.languages ?? [navigator.language ?? "en"];

  for (const lang of preferencias) {
    // Intentar match exacto (es-ES)
    if (localesDisponibles.includes(lang)) return lang;

    // Intentar match por idioma base (es-ES → es)
    const base = lang.split("-")[0];
    if (localesDisponibles.includes(base)) return base;
  }

  // Fallback al primer locale disponible
  return localesDisponibles[0];
}

const locale = detectarLocale(["es", "en", "fr"]);
// Si el usuario tiene ["es-ES", "en"] y los disponibles son ["es", "en"], retorna "es"
```

## Locale con la API de internacionalización

`Intl` usa el locale del browser automáticamente, pero puede forzarse uno explícito:

```js
// Usar el locale del usuario
const formato = new Intl.DateTimeFormat(navigator.language, {
  dateStyle: "long",
  timeStyle: "short",
});
formato.format(new Date()); // "8 de junio de 2026, 14:30" (en es-ES)

// Locale detectado por Intl (puede diferir si el OS tiene configuración diferente al browser)
const localeIntl = new Intl.DateTimeFormat().resolvedOptions().locale; // "es-ES"

// Formatear números según el locale del usuario
new Intl.NumberFormat(navigator.language).format(1234567.89);
// "1.234.567,89" en es-ES, "1,234,567.89" en en-US
```

## Cómo funciona por dentro

`navigator.language` se lee del encabezado `Accept-Language` que el browser envía al servidor, que a su vez refleja la configuración de idiomas del browser (en Chrome: Configuración → Idiomas). Cambiar el idioma en la configuración del browser actualiza `navigator.language` sin necesidad de recargar la página. El valor es de solo lectura desde JS — no puede modificarse.

`navigator.userAgent` se envía como cabecera HTTP en cada petición y también se expone en JS. El browser lo genera internamente combinando el motor, la versión y la plataforma en un formato heredado. En Firefox con resistencia a fingerprinting activada, `userAgent` puede ser genérico deliberadamente.

> [!tip]
> Para decisiones de locale en el servidor (SSR), usar el header `Accept-Language` de la petición HTTP en lugar de `navigator.language` — el header llega antes de que cualquier JS se ejecute y tiene el mismo valor. `navigator.language` es útil para lógica de locale en el cliente, por ejemplo para cargar dinámicamente un archivo de traducciones.

> [!warning]
> `navigator.platform` está **deprecado** — devuelve `"Win32"` en Windows de 64 bits, `"MacIntel"` en Apple Silicon, y valores inconsistentes en móviles. No usarlo para detectar el OS. La alternativa es `navigator.userAgentData.platform` (Chrome/Edge) o `navigator.userAgent` como último recurso con parsing defensivo.

## Notas relacionadas

- [[index|Navigator]] — visión general del objeto navigator
- [[02 onLine|onLine]] — conectividad de red
- [[../../07 Programación Asíncrona/index|Programación Asíncrona]] — Promises para las APIs asíncronas de navigator
