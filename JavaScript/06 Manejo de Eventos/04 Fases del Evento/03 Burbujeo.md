---
title: Burbujeo — El evento asciende desde el target
aliases:
  - burbujeo de eventos
  - event bubbling
  - bubbling phase
tags:
  - javascript
  - api/concepto
  - eventos
draft: false
order: 3
---

# Burbujeo

> [!definicion]
> El burbujeo es la tercera y última fase de propagación. Una vez que el evento ha sido procesado en el `target`, asciende elemento a elemento desde el `target` hasta `window`, ejecutando los listeners registrados con `capture: false` (el default) en cada ancestro que los tenga. Es el mecanismo que hace posible la delegación de eventos.

```js
// HTML: <div id="externo"><button id="btn">Clic</button></div>

const btn = document.querySelector('#btn');
const externo = document.querySelector('#externo');

btn.addEventListener('click', () => console.log('btn'));
externo.addEventListener('click', () => console.log('externo'));
document.addEventListener('click', () => console.log('document'));

// Clic en btn → imprime:
// btn
// externo
// document
```

## Orden de ejecución

El burbujeo asciende por el árbol DOM, ejecutando listeners en el orden:
`target` → padre directo → abuelo → … → `<body>` → `<html>` → `document` → `window`

Cada ancestro que tenga un listener registrado (sin `capture: true`) lo ejecuta en su turno.

## Eventos que no burbujean

La mayoría de eventos burbujean, pero hay excepciones:

| Evento | No burbujea | Alternativa que sí burbujea |
|---|---|---|
| `focus` | No | `focusin` |
| `blur` | No | `focusout` |
| `mouseenter` | No | `mouseover` |
| `mouseleave` | No | `mouseout` |
| `load` | No | — |
| `unload` | No | — |
| `scroll` (en elemento) | No (en la mayoría de contextos) | — |

`e.bubbles` indica si el evento específico burbujea (`true`) o no (`false`).

## El burbujeo y la delegación

El burbujeo es el fundamento de la [[05 Delegación de Eventos|delegación de eventos]]: registrar un listener en un ancestro y detectar el `e.target` para actuar solo sobre los descendientes relevantes. Esto es más eficiente que registrar un listener en cada elemento hijo.

```js
// Un listener en <ul> gestiona los clics de todos los <li> presentes y futuros
document.querySelector('ul').addEventListener('click', (e) => {
  const li = e.target.closest('li');
  if (li) resaltarElemento(li);
});
```

## Detener el burbujeo

`e.stopPropagation()` interrumpe el ascenso — los ancestros que queden por encima ya no reciben el evento. Se delega el detalle a [[03 stopPropagation y stopImmediatePropagation|stopPropagation]].

> [!info]
> El burbujeo es el comportamiento por defecto y el más usado en la práctica. Los listeners sin tercer argumento (o con `capture: false`) siempre operan en burbujeo. Solo se necesita `capture: true` cuando se requiere interceptar el evento antes de que llegue al target.

> [!warning]
> Para eventos que no burbujean (`focus`, `blur`, `mouseenter`, `mouseleave`), la delegación de eventos no funciona directamente. Usar las variantes `focusin`/`focusout` y `mouseover`/`mouseout` que sí burbujean, o registrar el listener directamente en cada elemento hijo.

## Notas relacionadas

- [[01 Captura|Captura]]
- [[02 Target|Fase Target]]
- [[04 Fases del Evento/index|Fases del Evento]]
- [[05 Delegación de Eventos|Delegación de Eventos]]
