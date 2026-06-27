---
title: APIs del Navegador — extensiones del browser al entorno JS
aliases:
  - APIs del Navegador
  - Browser APIs
  - Web APIs
tags:
  - javascript
  - api/concepto
  - browser
draft: false
order: 13
---

# APIs del Navegador

> [!definicion]
> Las **APIs del Navegador** son interfaces expuestas por el browser al entorno JS que extienden el lenguaje con acceso al hardware, al historial de navegación, al sistema de coordenadas del viewport, al canvas de renderizado 2D/3D, a los mecanismos de observación del DOM y al sistema de composición de la página. No forman parte de la especificación ECMAScript — las definen los estándares WHATWG, W3C y Khronos — y **solo están disponibles en el browser** (no en Node.js, salvo mediante polyfills).

```js
// Las APIs del browser viven como propiedades de window (o son clases globales)
window.navigator.userAgent;    // → información del agente de usuario
window.location.href;          // → URL actual
window.history.pushState(...); // → manipular el historial
window.performance.now();      // → timestamp de alta resolución
new IntersectionObserver(...); // → clase global del browser
```

## Subsecciones

| # | Nombre | Propósito |
|---|---|---|
| 01 | Window | El objeto global del browser: dimensiones, scroll, diálogos, comunicación entre ventanas |
| 02 | Navigator | Información del agente de usuario, permisos, geolocalización, conectividad, service workers |
| 03 | Location y URL | URL actual, navegación programática, parsing de URLs con `URL` y `URLSearchParams` |
| 04 | History | Historial de navegación, SPA routing con `pushState` / `replaceState`, evento `popstate` |
| 05 | Canvas API | Dibujo 2D y 3D en el browser: formas, trazados, estilos, transformaciones, exportación |
| 06 | Observers | APIs reactivas sin polling: `IntersectionObserver`, `MutationObserver`, `ResizeObserver` |
| 07 | Performance API | Métricas de rendimiento de alta resolución, Core Web Vitals, `PerformanceObserver` |
| 08 | Web Components | Estándares para elementos HTML personalizados: Custom Elements, Shadow DOM, Templates |

## Navegación

- [[01 Window/index|Window]] — propiedades y eventos del objeto global.
- [[02 Navigator/index|Navigator]] — `userAgent`, geolocalización, permisos, `navigator.onLine`.
- [[03 Location y URL/index|Location y URL]] — `location.href`, `URL`, `URLSearchParams`, hash routing.
- [[04 History/index|History]] — `pushState`, `replaceState`, `popstate`, SPA routing.
- [[05 Canvas API/index|Canvas API]] — `getContext("2d")`, formas, imágenes, animaciones.
- [[06 Observers/index|Observers]] — observers asíncronos para visibilidad, DOM y tamaño.
- [[07 Performance API|Performance API]] — `performance.now()`, `mark`, `measure`, `PerformanceObserver`.
- [[08 Web Components/index|Web Components]] — Custom Elements, Shadow DOM, HTML Templates.

> [!info]
> Estas APIs están definidas por el entorno del browser, no por el estándar ECMAScript. En Node.js, la mayoría no existen; las que se han añadido a Node (como `fetch`, `URL`, `setTimeout`) son implementaciones compatibles, no las mismas clases del browser. Al escribir código isomorfo (que corra en ambos), verificar la disponibilidad con guardas (`typeof window !== "undefined"`) o usar detección de features.

> [!tip]
> El objeto global `window` es el namespace de todas las APIs del browser. En módulos ES (`type="module"`), `this` en el top level no es `window` — usar `globalThis` para acceso universal al objeto global en cualquier entorno (browser, Node, Deno).

## Notas relacionadas

- [[../../JavaScript/07 Programación Asíncrona/index|Programación Asíncrona]] — la mayoría de las APIs del browser son asíncronas; entender el event loop es necesario para usarlas correctamente.
- [[../../JavaScript/06 Manejo de Eventos/index|Manejo de Eventos]] — `addEventListener` es el mecanismo de comunicación estándar entre las APIs del browser y el código de la aplicación.
- [[../../JavaScript/09 Almacenamiento en Cliente/index|Almacenamiento en Cliente]] — APIs de persistencia (`localStorage`, `sessionStorage`, `IndexedDB`, `Cookie`) que complementan las APIs de esta sección.
