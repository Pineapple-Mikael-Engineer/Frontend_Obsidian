---
title: Window — el objeto global del browser
aliases:
  - Window
  - objeto window
  - global object
tags:
  - javascript
  - api/objeto
  - browser
draft: false
order: 1
---

# `window`

> [!definicion]
> **`window`** es el objeto global del navegador: el contexto de ejecución de todo script JS que corre en una pestaña. Todas las variables globales declaradas con `var`, todas las APIs del browser (`document`, `fetch`, `setTimeout`, `localStorage`, `navigator`, `location`…) y todas las funciones globales son propiedades de `window`. Acceder a `window.algo` o simplemente a `algo` en el scope global es equivalente.

```js
// Variables globales en el scope de módulo son propiedades de window
var saludo = "hola";
window.saludo; // "hola"

// Las APIs del browser viven en window
window.document === document; // true
window.setTimeout === setTimeout; // true
window.navigator === navigator; // true

// En módulos ES (type="module"), las declaraciones top-level NO van a window
// Solo var y asignaciones directas: window.miVar = 42 sí van.
```

`window` también es el destino de eventos globales (`resize`, `scroll`, `online`, `offline`, `beforeunload`) y el contexto `this` en funciones no-estrictas ejecutadas en el scope global.

## Bloques de esta sección

- [[01 Dimensiones y Scroll|Dimensiones y Scroll]] — `innerWidth`, `scrollY`, `scrollTo`, `scrollBy` y recetas de scroll.
- [[02 open y close|open y close]] — abrir y cerrar ventanas/pestañas, seguridad con `noopener` y `postMessage`.
- [[03 alert, confirm, prompt|alert, confirm, prompt]] — diálogos bloqueantes nativos, por qué evitarlos y la alternativa con `<dialog>`.

## Propiedades y métodos notables

| Propiedad / Método | Descripción |
|---|---|
| `window.document` | El `Document` de la pestaña actual |
| `window.location` | URL actual — leer y navegar |
| `window.history` | Historial de navegación de la pestaña |
| `window.navigator` | Info del agente de usuario y APIs del sistema |
| `window.localStorage` / `window.sessionStorage` | Web Storage por origen / por sesión |
| `window.innerWidth` / `window.innerHeight` | Dimensiones del viewport |
| `window.scrollX` / `window.scrollY` | Posición del scroll actual |
| `window.open(url)` | Abre una nueva pestaña o ventana popup |
| `window.close()` | Cierra la ventana (solo si fue abierta con `open`) |
| `window.alert` / `window.confirm` / `window.prompt` | Diálogos modales bloqueantes |
| `window.setTimeout` / `window.setInterval` | Temporizadores del Event Loop |
| `window.fetch` | API de red basada en Promises |
| `window.crypto` | Criptografía — `crypto.randomUUID()`, `crypto.subtle` |
| `window.performance` | Métricas de rendimiento — `performance.now()` |
| `window.matchMedia` | Evaluar media queries desde JS |

## Cómo funciona por dentro

Cada pestaña del navegador tiene su propio proceso de rendering (en Chromium desde 2018, modelo de aislamiento por sitio). Dentro de ese proceso, el motor JS (V8 en Chrome/Edge, SpiderMonkey en Firefox) crea un contexto de ejecución cuyo objeto global es precisamente `window`. Cuando un script accede a una variable sin declarar en scope local, el motor sube por la cadena de scopes hasta llegar a `window`. En Web Workers no existe `window` — el objeto global es `self` (un `WorkerGlobalScope`), lo que explica por qué código que asume `window` falla en workers.

> [!tip]
> En módulos ES (`<script type="module">`) el scope top-level no es `window` — las declaraciones `const`/`let`/`function` son locales al módulo. Para exponer algo globalmente desde un módulo hay que asignar explícitamente: `window.miUtil = function() {}`.

> [!warning]
> Modificar `window` con propiedades propias (polyfills, variables globales de app) puede colisionar con futuras APIs nativas o con librerías de terceros. Usar namespacing (`window.MiApp = {}`) o módulos ES evita esta clase de colisiones.

## Notas relacionadas

- [[../02 Navigator/index|Navigator]] — agente de usuario, geolocation, clipboard y más APIs del sistema
- [[../03 Location y URL/index|Location y URL]] — navegación, URL parsing y el objeto `location`
- [[../04 History/index|History]] — historial de navegación y la History API
