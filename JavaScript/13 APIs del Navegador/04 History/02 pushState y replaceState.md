---
title: history.pushState y replaceState
aliases:
  - pushState
  - replaceState
  - history.pushState
  - history.replaceState
  - history.state
tags:
  - javascript
  - api/objeto
  - browser
draft: false
order: 2
---

# `history.pushState()` y `history.replaceState()`

> [!definicion]
> `history.pushState(state, title, url)` añade una nueva entrada al historial de la sesión y cambia la URL visible **sin recargar la página ni hacer ninguna petición HTTP**. `history.replaceState(state, title, url)` hace lo mismo pero reemplaza la entrada actual en lugar de añadir una nueva. Son el núcleo del routing de SPAs: permiten que la URL refleje el estado de la aplicación mientras el JS controla qué se renderiza.

```js
// Navegar a "/perfil/42" sin recargar — añade entrada al historial
history.pushState({ userId: 42, vista: "perfil" }, "", "/perfil/42");

// Actualizar la URL del filtro de búsqueda — reemplaza la entrada actual
history.replaceState({ q: "typescript", page: 1 }, "", "?q=typescript&page=1");

// Leer el state de la entrada actual
console.log(history.state); // { q: "typescript", page: 1 }
```

## Firma y parámetros

```js
history.pushState(state, title, url)
history.replaceState(state, title, url)
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `state` | `any` (serializable) | Objeto JS guardado con la entrada del historial. Disponible en `history.state` y en `e.state` del evento `popstate`. Límite: ~2MB en Chrome/Firefox. Debe ser serializable con structured clone. |
| `title` | `string` | Ignorado por todos los browsers actuales. Por convención se pasa `""` o el título de la página. |
| `url` | `string \| URL \| null` | La nueva URL. Debe ser del mismo origen. Si empieza con `/` es absoluta; si no, es relativa a la URL actual. `null` mantiene la URL actual. |

## `pushState` vs `replaceState`

| Método | Efecto sobre el historial | Cuándo usar |
|---|---|---|
| `pushState` | Añade nueva entrada — `history.length` aumenta en 1 | Navegación entre vistas (el usuario debe poder volver) |
| `replaceState` | Reemplaza la entrada actual — `history.length` no cambia | Actualizar URL de filtros/búsqueda sin crear entradas extra |

## El objeto `state`

El `state` es el mecanismo para adjuntar datos a una entrada del historial. Cuando el usuario navega atrás/adelante, `e.state` en el evento `popstate` contiene el state guardado con esa entrada, lo que permite restaurar el estado de la vista sin necesidad de parsear la URL:

```js
// Al navegar a una vista, guardar el state necesario para restaurarla
function navegarA(ruta, datos) {
  history.pushState(datos, "", ruta);
  renderizarVista(ruta, datos);
}

navegarA("/articulo/123", { id: 123, titulo: "Intro a JS", scroll: 0 });

// Al navegar atrás/adelante, restaurar desde el state
window.addEventListener("popstate", (e) => {
  if (e.state) {
    renderizarVista(location.pathname, e.state);
  } else {
    // state es null: la entrada fue creada por el browser (carga inicial)
    renderizarVista(location.pathname, {});
  }
});
```

## La URL debe ser del mismo origen

`pushState` y `replaceState` solo aceptan URLs del mismo origen que la página actual. Intentar pasar una URL de otro origen lanza un `SecurityError`:

```js
// Correcto — mismo origen
history.pushState({}, "", "/nueva-ruta");            // ruta absoluta
history.pushState({}, "", "subpagina");              // ruta relativa
history.pushState({}, "", "?filtro=activo");         // solo query string
history.pushState({}, "", "#seccion");               // solo hash
history.pushState({}, "", "https://misitio.com/x"); // mismo origen, ok

// Lanza SecurityError — origen distinto
history.pushState({}, "", "https://otro-sitio.com/ruta");
```

## Diferencia con `location.href`

`pushState` cambia la URL sin activar ninguna carga de página. `location.href = "/nueva"` dispara una navegación completa:

```js
// location.href: recarga la app completa, pierde el estado JS
location.href = "/perfil";

// pushState: solo cambia la URL visible, el JS sigue ejecutándose
history.pushState({ vista: "perfil" }, "", "/perfil");
renderizarPerfil(); // la app decide qué mostrar
```

## `pushState` no dispara `popstate`

Llamar a `pushState` o `replaceState` no dispara el evento `popstate`. El evento solo se dispara cuando el usuario (o `history.go()`, `back()`, `forward()`) navega por el historial. La app debe llamar explícitamente a la función de renderizado al hacer `pushState`:

```js
function navegar(ruta, state = {}) {
  history.pushState(state, "", ruta);
  renderizarRuta(ruta, state); // llamada explícita — popstate NO se disparará
}
```

## Cómo funciona por dentro

`pushState` y `replaceState` se comunican directamente con el proceso de historial del browser (el mismo que gestiona los botones de atrás/adelante). La "navegación" que realizan es de tipo `same-document` (sin petición HTTP): solo actualiza el puntero del historial, la barra de direcciones del browser y el state asociado a la entrada. El motor de JS no se reinicia, el DOM permanece intacto y no se dispara ningún evento de carga (`load`, `DOMContentLoaded`). El state se serializa con el algoritmo structured clone (el mismo que usa `postMessage` y `IndexedDB`), por lo que puede contener tipos complejos como `Map`, `Date` o `ArrayBuffer`, pero no funciones ni nodos del DOM.

> [!tip]
> Para filtros de búsqueda o paginación donde cada cambio no debe crear una entrada extra en el historial (el usuario no necesita "volver" a cada valor intermedio), usar `replaceState`. Para navegación entre vistas distintas donde el usuario espera poder usar el botón "atrás", usar `pushState`. La regla: si la URL representa un estado que merece una entrada en el historial, `pushState`; si es una actualización del estado actual, `replaceState`.

> [!warning]
> El límite de tamaño del objeto `state` es de aproximadamente 2MB en Chrome y Firefox. Pasar objetos grandes al state puede lanzar una excepción `QuotaExceededError`. Guardar en el state solo los identificadores necesarios para restaurar la vista (IDs, nombres de vista), no datos completos de respuestas de API — esos deben almacenarse en caché de la app o re-fetched al navegar.

## Notas relacionadas

- [[03 Evento popstate|Evento popstate]] — cómo responder a la navegación del usuario con atrás/adelante
- [[01 back, forward, go|back, forward, go]] — navegación programática por el historial existente
- [[../03 Location y URL/03 URLSearchParams|URLSearchParams]] — construir la query string para pasar a `pushState`
