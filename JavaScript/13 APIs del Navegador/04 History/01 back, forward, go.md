---
title: history.back, forward y go
aliases:
  - history.back
  - history.forward
  - history.go
  - history.length
  - scrollRestoration
tags:
  - javascript
  - api/objeto
  - browser
draft: false
order: 1
---

# `history.back()`, `forward()` y `go()`

> [!definicion]
> `window.history` expone métodos para navegar programáticamente por el historial de la sesión actual: `back()` retrocede una entrada, `forward()` avanza una entrada y `go(delta)` salta un número arbitrario de posiciones. Estas funciones son el equivalente programático de los botones de navegación del browser. La navegación es **asíncrona** — la URL cambia y el evento `popstate` se dispara cuando la transición completa, no de forma síncrona al llamar al método.

```js
history.back();     // retrocede una entrada — equivale a go(-1)
history.forward();  // avanza una entrada — equivale a go(1)
history.go(-1);     // retroceder 1 (equivale a back())
history.go(1);      // avanzar 1 (equivale a forward())
history.go(-3);     // retroceder 3 entradas de golpe
history.go(0);      // recarga la página actual
history.go();       // sin argumento: recarga la página actual
```

## Propiedades de `history`

| Propiedad | Tipo | Descripción |
|---|---|---|
| `length` | `number` | Número de entradas en el historial de la sesión, incluyendo la actual. Máximo 50 en la mayoría de browsers. |
| `state` | `any` | El objeto state guardado con la entrada actual del historial, o `null` si no se guardó. |
| `scrollRestoration` | `"auto" \| "manual"` | Controla si el browser restaura la posición de scroll al navegar. |

## `history.length`

Indica cuántas entradas hay en el historial de la sesión actual (incluyendo la página actual). Es útil para saber si se puede navegar atrás:

```js
// Verificar si hay páginas previas antes de invocar back()
if (history.length > 1) {
  history.back();
} else {
  // No hay historial previo — ir a la página de inicio
  location.replace("/");
}
```

## `history.scrollRestoration`

Controla si el browser restaura automáticamente la posición de scroll cuando el usuario navega hacia atrás o adelante:

```js
// "auto" (valor por defecto): el browser restaura la posición de scroll
history.scrollRestoration; // "auto"

// "manual": la app gestiona el scroll — necesario en SPAs con transiciones animadas
history.scrollRestoration = "manual";

window.addEventListener("popstate", (e) => {
  // El browser no moverá el scroll — la app decide cómo hacerlo
  const posicionGuardada = e.state?.scrollY ?? 0;
  window.scrollTo({ top: posicionGuardada, behavior: "smooth" });
});
```

En SPAs con animaciones de transición entre vistas, `"auto"` suele producir saltos de scroll indeseados. Configurar `"manual"` y guardar `window.scrollY` en el state de `pushState` permite restaurar la posición de forma controlada.

## Asincronía de la navegación

Los métodos `back()`, `forward()` y `go()` son **asíncronos**. La llamada retorna inmediatamente, pero la navegación y el evento `popstate` ocurren después. No existe una promesa ni callback — el resultado se observa en el listener de `popstate`:

```js
history.back(); // la URL todavía no ha cambiado aquí
// Inmediatamente después de este punto, location.pathname sigue siendo el actual

// La respuesta llega aquí, de forma asíncrona:
window.addEventListener("popstate", (e) => {
  console.log("Navegación completada a:", location.pathname);
  console.log("State:", e.state);
});
```

Si el delta solicitado (`go(delta)`) no es alcanzable (por ejemplo, `go(-10)` cuando solo hay 3 entradas previas), el método no hace nada y no dispara `popstate`.

## Cómo funciona por dentro

El historial de la sesión es una lista lineal de entradas mantenida por el proceso del browser (no por el renderer de JS). `back()`, `forward()` y `go()` envían un mensaje al proceso de historial del browser solicitando mover el puntero de la entrada actual. Una vez que el proceso confirma el movimiento (tras resolver la cache, actualizar la barra de direcciones y restaurar el estado), el renderer recibe la señal para disparar el evento `popstate` en el `window`. Por este motivo la navegación es asíncrona: implica IPC entre procesos del browser.

> [!tip]
> `history.go(0)` es una alternativa a `location.reload()` con un comportamiento ligeramente diferente: recarga la página actual respetando el caché de la misma forma que `reload()` sin argumentos. Para una SPA donde `popstate` controla el rendering, `go(0)` puede usarse para forzar un re-render sin recargar la app.

> [!warning]
> No asumir que `history.back()` siempre llevará al usuario a la página anterior de tu app. Si el usuario llegó a la página actual desde un dominio externo, `back()` puede navegar fuera de la app. Si el destino importa, usar `history.length` y `document.referrer` para tomar una decisión informada, o redirigir explícitamente con `location.replace()` en lugar de confiar en `back()`.

## Notas relacionadas

- [[02 pushState y replaceState|pushState y replaceState]] — añadir entradas al historial programáticamente
- [[03 Evento popstate|Evento popstate]] — el evento que se dispara al completar la navegación
- [[../03 Location y URL/02 reload, assign, replace|reload, assign, replace]] — recarga y navegación con `location`
