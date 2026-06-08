---
title: El Objeto Event — Índice
aliases:
  - Event object
  - objeto Event
tags:
  - javascript
  - api/concepto
  - eventos
draft: false
---

# El Objeto Event

> [!definicion]
> Cuando el navegador dispara un evento, construye un objeto que implementa la interfaz `Event` (o una subinterfaz más específica) y lo pasa como primer argumento a cada listener. Este objeto describe el evento: qué tipo es, qué elemento lo originó, en qué elemento se está procesando ahora, si se puede cancelar, y propiedades adicionales según el tipo (coordenadas para eventos de ratón, tecla pulsada para teclado, etc.).

```js
document.addEventListener('click', (e) => {
  console.log(e.type);          // 'click'
  console.log(e.target);        // el elemento clicado
  console.log(e.currentTarget); // el elemento con el listener (document)
  console.log(e.bubbles);       // true
  console.log(e.cancelable);    // true
});
```

## Propiedades universales de Event

Presentes en todos los tipos de evento:

| Propiedad / Método | Tipo | Descripción |
|---|---|---|
| `type` | string | Nombre del evento: `'click'`, `'keydown'`, `'submit'` |
| `target` | EventTarget | Elemento que originó el evento |
| `currentTarget` | EventTarget | Elemento donde está registrado el listener activo |
| `bubbles` | boolean | `true` si el evento sube por el árbol DOM |
| `cancelable` | boolean | `true` si `preventDefault()` tiene efecto |
| `defaultPrevented` | boolean | `true` si ya se llamó `preventDefault()` |
| `eventPhase` | number | 1 = captura, 2 = target, 3 = burbujeo |
| `timeStamp` | number | ms desde el origen de la página (performance.now) |
| `isTrusted` | boolean | `true` si fue generado por el usuario, `false` si fue creado con JS |
| `preventDefault()` | — | Cancela la acción por defecto del navegador |
| `stopPropagation()` | — | Detiene la propagación (burbujeo o captura) |
| `stopImmediatePropagation()` | — | Detiene propagación y cancela los demás listeners del nodo actual |

## Jerarquía de subinterfaces

```
Event
├── UIEvent
│   ├── MouseEvent → click, mousedown, mousemove, mouseover…
│   ├── KeyboardEvent → keydown, keyup
│   ├── TouchEvent → touchstart, touchmove, touchend
│   └── WheelEvent → wheel
├── FocusEvent → focus, blur, focusin, focusout
├── InputEvent → input
└── CustomEvent → eventos personalizados
```

## Subsecciones

| # | Nota | Cubre |
|---|---|---|
| 01 | target vs currentTarget | Diferencia entre el origen y el receptor del listener |
| 02 | preventDefault | Cancelar la acción por defecto, `cancelable`, casos de uso |
| 03 | stopPropagation y stopImmediatePropagation | Detener la propagación, comparativa, antipatrones |
| 04 | Propiedades Específicas (clientX, key) | MouseEvent, KeyboardEvent, TouchEvent, WheelEvent, InputEvent |

## Notas relacionadas

- [[01 target vs currentTarget|target vs currentTarget]]
- [[02 preventDefault|preventDefault]]
- [[03 stopPropagation y stopImmediatePropagation|stopPropagation]]
- [[04 Propiedades Específicas (clientX, key)|Propiedades Específicas]]
- [[04 Fases del Evento/index|Fases del Evento]]
- [[05 Delegación de Eventos|Delegación de Eventos]]
