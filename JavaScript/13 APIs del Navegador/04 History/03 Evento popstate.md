---
title: Evento popstate
aliases:
  - evento popstate
  - popstate
  - PopStateEvent
tags:
  - javascript
  - api/evento
  - browser
draft: false
order: 3
---

# Evento `popstate`

> [!definicion]
> El evento `popstate` se dispara en `window` cuando la entrada activa del historial de la sesión cambia como resultado de la navegación del usuario (botones de atrás/adelante) o de una llamada a `history.go()`, `history.back()` o `history.forward()`. **No se dispara** al llamar a `history.pushState()` o `history.replaceState()`. `e.state` contiene el objeto guardado con esa entrada del historial, lo que permite restaurar el estado de la vista sin parsear la URL.

```js
window.addEventListener("popstate", (e) => {
  const ruta = location.pathname;
  const state = e.state; // objeto guardado con pushState, o null
  renderizarRuta(ruta, state);
});
```

## Propiedades del `PopStateEvent`

| Propiedad | Tipo | Descripción |
|---|---|---|
| `state` | `any \| null` | El objeto state guardado con esa entrada del historial mediante `pushState` o `replaceState`. `null` si la entrada fue creada por el browser (carga inicial o navegación por el browser antes de usar `pushState`). |
| `hasUAVisualTransition` | `boolean` | `true` si el browser está realizando una transición visual nativa (View Transitions API). Nuevo en 2024. |

## Cuándo se dispara y cuándo no

| Acción | Dispara `popstate` |
|---|---|
| `history.back()` | Sí |
| `history.forward()` | Sí |
| `history.go(delta)` (distinto de 0) | Sí |
| Botón "atrás" del browser | Sí |
| Botón "adelante" del browser | Sí |
| `history.pushState()` | No |
| `history.replaceState()` | No |
| `location.href = url` (misma página, distinto hash) | Sí (también dispara `hashchange`) |
| Carga inicial de la página | No |

## El ciclo completo del router SPA

Un router SPA correcto necesita cubrir dos casos de navegación distintos: la navegación iniciada por la app (clic en un enlace interno) y la navegación iniciada por el browser (botones de atrás/adelante). El evento `popstate` cierra el segundo caso:

```js
// --- PIEZA 1: interceptar clics en enlaces internos ---
document.addEventListener("click", (e) => {
  const enlace = e.target.closest("a[href]");
  if (!enlace) return;

  const url = new URL(enlace.href);
  // Solo interceptar enlaces del mismo origen
  if (url.origin !== location.origin) return;

  e.preventDefault();
  history.pushState({ from: "click" }, "", url.pathname + url.search);
  renderizarRuta(url.pathname, url.search);
});

// --- PIEZA 2: responder a la navegación del browser (atrás/adelante) ---
window.addEventListener("popstate", (e) => {
  renderizarRuta(location.pathname, location.search, e.state);
});

// --- PIEZA 3: renderizar en la carga inicial (popstate NO se dispara) ---
document.addEventListener("DOMContentLoaded", () => {
  renderizarRuta(location.pathname, location.search, history.state);
});
```

## El estado inicial no dispara `popstate`

Cuando el usuario carga la página por primera vez (o la recarga), `popstate` no se dispara. El renderizado inicial debe realizarse en `DOMContentLoaded` o de forma síncrona al evaluar el script. `history.state` puede ser `null` en la carga inicial, o contener el state guardado si el browser restauró la sesión:

```js
// Renderizado inicial — sin popstate
document.addEventListener("DOMContentLoaded", () => {
  const stateInicial = history.state ?? {};
  renderizarRuta(location.pathname, location.search, stateInicial);
});
```

## `hashchange` y routing por hash

Si la URL cambia solo en el hash (`#/ruta`), el browser dispara tanto `popstate` como el evento `hashchange`. El routing por hash (hash routing) es una técnica más antigua que no requiere `pushState` y funciona en entornos sin server-side routing configurado:

```js
// Routing por hash — URL: /app#/perfil/42
window.addEventListener("hashchange", (e) => {
  const hash = location.hash; // "#/perfil/42"
  const ruta = hash.slice(1); // "/perfil/42"
  renderizarRutaHash(ruta);
});

// También se puede usar popstate para hash changes
window.addEventListener("popstate", (e) => {
  if (location.hash) {
    renderizarRutaHash(location.hash.slice(1));
  }
});
```

El routing por hash no requiere configuración del servidor (todas las peticiones llegan a `index.html`) pero produce URLs menos limpias (`/app#/perfil` en lugar de `/perfil`). El routing con `pushState` produce URLs limpias pero requiere que el servidor devuelva `index.html` para cualquier ruta.

## Receta: router SPA completo

Router mínimo con `pushState` + `popstate` + soporte a módulos de vista:

```js
const rutas = {
  "/": () => import("./vistas/Inicio.js"),
  "/perfil": () => import("./vistas/Perfil.js"),
  "/articulos": () => import("./vistas/Articulos.js"),
};

async function renderizarRuta(ruta, state = {}) {
  const cargador = rutas[ruta] ?? rutas["/"];
  const { default: Vista } = await cargador();
  const contenedor = document.getElementById("app");
  contenedor.innerHTML = "";
  contenedor.appendChild(new Vista(state).render());
}

function navegar(ruta, state = {}) {
  history.pushState(state, "", ruta);
  renderizarRuta(ruta, state);
}

// Navegación del browser
window.addEventListener("popstate", (e) => {
  renderizarRuta(location.pathname, e.state ?? {});
});

// Interceptar enlaces
document.addEventListener("click", (e) => {
  const a = e.target.closest("a[href]");
  if (!a || new URL(a.href).origin !== location.origin) return;
  e.preventDefault();
  navegar(a.pathname, {});
});

// Carga inicial
document.addEventListener("DOMContentLoaded", () => {
  renderizarRuta(location.pathname, history.state ?? {});
});

// Exponer navegar globalmente para uso desde la app
window.__navegar = navegar;
```

## Cómo funciona por dentro

Cuando el usuario pulsa el botón de "atrás", el browser mueve el puntero del historial de la sesión a la entrada anterior, actualiza `location` con la URL de esa entrada y, una vez completada la actualización interna, encola un evento `PopStateEvent` en el `window` del renderer. El evento se procesa de forma asíncrona respecto al clic del usuario — hay un pequeño retardo (generalmente imperceptible) entre el clic y el disparo de `popstate`. Esto es especialmente relevante si se intenta leer `location.pathname` sincrónicamente justo después de llamar a `history.back()`: la URL todavía no habrá cambiado. La URL correcta solo es fiable dentro del listener de `popstate`.

> [!tip]
> Para compatibilidad con la carga inicial y con la navegación del browser, estructurar el renderizado en una función `renderizarRuta(ruta, state)` y llamarla tanto en `DOMContentLoaded` (con `location.pathname` y `history.state`) como en el listener de `popstate` (con `location.pathname` y `e.state`). Esto garantiza que la lógica de rendering es la misma independientemente de cómo llegó el usuario a la vista.

> [!warning]
> En algunos browsers (Chrome históricamente), al cargar la página `popstate` se podía disparar con `state: null` inmediatamente. Este comportamiento fue eliminado de la especificación en 2011 pero algunos polyfills antiguos lo imitan. Si el código maneja `popstate` en la carga inicial, verificar que el state no es `null` antes de intentar restaurar desde él, o usar `DOMContentLoaded` para el renderizado inicial en lugar de `popstate`.

## Notas relacionadas

- [[02 pushState y replaceState|pushState y replaceState]] — añadir entradas al historial sin recargar
- [[01 back, forward, go|back, forward, go]] — los métodos que disparan `popstate`
- [[../03 Location y URL/03 URLSearchParams|URLSearchParams]] — leer los params de la URL dentro de `popstate`
- [[../03 Location y URL/index|Location y URL]] — leer `location.pathname` y `location.search` dentro del listener
