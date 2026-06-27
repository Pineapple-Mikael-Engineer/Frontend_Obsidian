---
title: preventDefault — Cancelar la acción por defecto del navegador
aliases:
  - preventDefault
  - e.preventDefault
  - Event.preventDefault
tags:
  - javascript
  - api/metodo
  - eventos
objeto: Event
tipo: metodo
retorna: undefined
muta: false
asincrono: false
draft: false
order: 2
---

# preventDefault

> [!definicion]
> `e.preventDefault()` cancela la acción que el navegador ejecutaría por defecto al producirse el evento: seguir un enlace, enviar un formulario, desplazarse con la rueda, abrir el menú contextual, etc. No detiene la propagación del evento — el burbujeo continúa normalmente. Solo tiene efecto si `e.cancelable === true`.

```js
const enlace = document.querySelector('a#interno');

enlace.addEventListener('click', (e) => {
  e.preventDefault();   // el navegador no navega a href
  router.navegar(enlace.href);  // navegación manual de SPA
});
```

## Acciones cancelables frecuentes

| Evento | Elemento | Acción por defecto cancelable |
|---|---|---|
| `click` | `<a href>` | Navegar a la URL |
| `submit` | `<form>` | Enviar el formulario y recargar/redirigir |
| `keydown` | input, textarea | Insertar el carácter correspondiente |
| `wheel` | cualquier elemento | Desplazar la página o el elemento |
| `touchmove` | móvil | Scroll táctil |
| `contextmenu` | cualquier elemento | Mostrar menú contextual del navegador |
| `dragstart` | elemento draggable | Iniciar drag del navegador |
| `beforeunload` | window | Cerrar/navegar fuera de la página |

## `e.cancelable` y `e.defaultPrevented`

`e.cancelable` indica si el evento admite cancelación — no todos los eventos son cancelables. Llamar `preventDefault()` en un evento con `cancelable === false` no tiene efecto.

`e.defaultPrevented` es `true` si ya se llamó `preventDefault()` en este evento, lo que permite que handlers posteriores detecten si ya fue cancelado.

```js
documento.addEventListener('click', (e) => {
  if (!e.cancelable) return;
  e.preventDefault();
});

documento.addEventListener('click', (e) => {
  if (e.defaultPrevented) {
    console.log('la acción por defecto ya fue cancelada');
  }
});
```

## Receta: validación custom de formulario

```js
const form = document.querySelector('#checkout');

form.addEventListener('submit', (e) => {
  e.preventDefault();  // evitar el envío nativo

  const errores = validar(form);
  if (errores.length > 0) {
    mostrarErrores(errores);
    return;
  }

  // envío manual con fetch
  fetch('/api/pedido', {
    method: 'POST',
    body: new FormData(form),
  }).then(procesarRespuesta);
});
```

## Receta: interceptar links en una SPA

```js
document.addEventListener('click', (e) => {
  const enlace = e.target.closest('a[href]');
  if (!enlace) return;
  if (enlace.origin !== location.origin) return;  // externo: dejar pasar

  e.preventDefault();
  history.pushState({}, '', enlace.pathname);
  renderizarRuta(enlace.pathname);
});
```

## Receta: deshabilitar el menú contextual en un canvas

```js
canvas.addEventListener('contextmenu', (e) => {
  e.preventDefault();
  mostrarMenuPropio(e.clientX, e.clientY);
});
```

> [!warning]
> En listeners con `passive: true`, `preventDefault()` se ignora silenciosamente (o el navegador emite una advertencia). Esto afecta especialmente a `touchmove` y `wheel` cuando se necesita bloquear el scroll — en ese caso no usar `passive: true`.

> [!tip]
> `preventDefault()` no impide que otros listeners en el mismo elemento o en ancestros reciban el evento. Si además se quiere detener la propagación, hay que llamar también a `stopPropagation()` — son mecanismos ortogonales.

## Notas relacionadas

- [[03 stopPropagation y stopImmediatePropagation|stopPropagation]]
- [[03 El Objeto Event/index|El Objeto Event]]
- [[02 Opciones (once, passive, capture)|Opciones (once, passive, capture)]]
