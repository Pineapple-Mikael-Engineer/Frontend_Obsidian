---
title: History API — visión general
aliases:
  - History API
  - historial de navegación
  - SPA routing
tags:
  - javascript
  - api/objeto
  - browser
draft: false
order: 4
---

# History API

> [!definicion]
> La History API (`window.history`) permite leer y manipular el historial de navegación del browser desde JavaScript. Sus dos capacidades clave son: (1) navegar programáticamente por el historial existente (`back()`, `forward()`, `go()`), y (2) añadir o modificar entradas del historial **sin recargar la página** (`pushState()`, `replaceState()`). Esta segunda capacidad es la base del routing en las SPAs modernas: la URL visible cambia, el browser registra la entrada en el historial, pero el JS de la app controla qué se renderiza.

```js
// Navegar por el historial
history.back();     // equivalente al botón "atrás"
history.forward();  // equivalente al botón "adelante"
history.go(-2);     // retroceder dos páginas

// Cambiar la URL sin recargar (routing de SPA)
history.pushState({ vista: "perfil", userId: 42 }, "", "/perfil/42");
history.replaceState({ vista: "inicio" }, "", "/");

// Leer el estado de la entrada actual
console.log(history.state); // { vista: "inicio" }
console.log(history.length); // número de entradas en el historial
```

## Contenido de esta sección

- [[01 back, forward, go|back, forward, go]] — navegación programática por el historial existente y las propiedades `length` y `scrollRestoration`.
- [[02 pushState y replaceState|pushState y replaceState]] — añadir y reemplazar entradas del historial sin recargar; el objeto `state` y los límites de URL.
- [[03 Evento popstate|Evento popstate]] — el evento que dispara el browser al navegar hacia atrás/adelante; la pieza que cierra el ciclo del router SPA.

## El ciclo del router SPA

La History API no opera de forma aislada. El routing de una SPA requiere tres piezas coordinadas:

1. **Interceptar los clics en `<a>`** — `e.preventDefault()` + `history.pushState()` en lugar de dejar que el browser navegue.
2. **Renderizar la vista** — la app lee `location.pathname` y decide qué mostrar.
3. **Escuchar `popstate`** — cuando el usuario pulsa "atrás" o "adelante", el browser dispara `popstate` y la app vuelve a leer `location.pathname` para renderizar la vista correcta.

Sin el listener de `popstate`, los botones de navegación del browser rompen la SPA (la URL cambia pero la vista no se actualiza).

> [!tip]
> `history.length` incluye la entrada actual. Su valor máximo es 50 en la mayoría de browsers. En una SPA que usa `pushState` agresivamente (cada keystroke de búsqueda, por ejemplo), `replaceState` es más apropiado para no contaminar el historial con estados intermedios.

> [!warning]
> La History API no persiste entre sesiones del browser — el historial de la sesión se pierde al cerrar la pestaña. Para persistir el estado de la aplicación entre sesiones, combinar con `localStorage` o `sessionStorage`. El objeto `state` de `pushState` tampoco se persiste a disco en todos los browsers — al recargar la página, `history.state` puede ser `null`.

## Notas relacionadas

- [[../03 Location y URL/index|Location y URL]] — `location.pathname`, `location.search` y `URLSearchParams` para leer la URL actual
- [[../03 Location y URL/03 URLSearchParams|URLSearchParams]] — sincronizar filtros con la URL sin recargar
- [[../../07 Programación Asíncrona/index|Programación Asíncrona]] — fetch de datos al renderizar la vista en el router SPA
