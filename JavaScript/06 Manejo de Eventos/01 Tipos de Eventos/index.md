---
title: Tipos de Eventos — Índice
aliases:
  - Categorías de Eventos DOM
  - Event Types Index
tags:
  - javascript
  - api/evento
  - eventos
draft: false
order: 1
---

# Tipos de Eventos

> [!definicion]
> Los eventos del DOM se agrupan por la fuente o el mecanismo que los dispara. Cada grupo tiene su propia interfaz que hereda de `Event` y añade propiedades específicas al contexto. Conocer qué interfaz usa cada grupo determina qué propiedades estarán disponibles en el objeto `e` del handler.

El modelo de eventos del navegador define más de 200 tipos de eventos. La especificación DOM Living Standard los organiza en familias según su origen: interacción del usuario con dispositivos de entrada (ratón, teclado), estado del documento y la ventana, interacción con formularios, medios, animaciones CSS, y mecanismos del portapapeles y el arrastre.

## Grupos de eventos

| Grupo | Ejemplos de eventos | Interfaz principal |
|---|---|---|
| Ratón | click, mousedown, mousemove, mouseover | MouseEvent |
| Teclado | keydown, keyup, keypress | KeyboardEvent |
| Formulario | submit, change, input, focus, blur | Event / FocusEvent |
| Ventana y documento | DOMContentLoaded, load, resize, scroll | Event / UIEvent |
| Portapapeles | copy, cut, paste | ClipboardEvent |
| Drag and Drop | dragstart, dragover, drop | DragEvent |
| Medios | play, pause, timeupdate, ended | Event |
| Animación y transición CSS | animationend, transitionend | AnimationEvent / TransitionEvent |

## Jerarquía de interfaces

Todos los eventos siguen la cadena de herencia `EventTarget → Event → [interfaz específica]`. Las propiedades universales como `type`, `target`, `currentTarget`, `bubbles`, `cancelable`, `defaultPrevented` y los métodos `preventDefault()` y `stopPropagation()` viven en la base `Event` y están disponibles en todos los grupos.

Las interfaces especializadas extienden esta base con propiedades relevantes al contexto:
- `UIEvent` agrega `view` (referencia al `window`) y `detail` (número de clicks en `dblclick`).
- `MouseEvent` hereda de `UIEvent` y agrega `clientX/Y`, `button`, `relatedTarget`, etc.
- `KeyboardEvent` hereda de `UIEvent` y agrega `key`, `code`, `repeat`.
- `DragEvent` hereda de `MouseEvent` y agrega `dataTransfer`.
- `ClipboardEvent` hereda de `Event` y agrega `clipboardData`.
- `AnimationEvent` y `TransitionEvent` heredan de `Event` y agregan el nombre de la animación/propiedad.

## Notas relacionadas

- [[01 Eventos de Ratón]]
- [[02 Eventos de Teclado]]
- [[03 Eventos de Formulario]]
- [[04 Eventos de Ventana y Documento]]
- [[05 Eventos de Portapapeles]]
- [[06 Drag and Drop]]
- [[07 Eventos de Medios]]
- [[08 Eventos de Animación y Transición CSS]]
