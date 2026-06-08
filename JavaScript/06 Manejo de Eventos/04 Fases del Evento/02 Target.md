---
title: Fase Target — El evento llega al elemento de origen
aliases:
  - fase target
  - AT_TARGET
  - eventPhase 2
tags:
  - javascript
  - api/concepto
  - eventos
draft: false
---

# Target

> [!definicion]
> La fase target es la segunda de las tres fases de propagación. El evento alcanza el elemento que lo originó (`e.target`) y ejecuta todos los listeners registrados en ese elemento para ese tipo de evento, sin distinción de si están marcados como captura o burbujeo. `e.eventPhase === 2` durante esta fase.

```js
const btn = document.querySelector('#btn');

// Ambos listeners del mismo btn se ejecutan en la fase target,
// en orden de registro, independientemente del flag capture
btn.addEventListener('click', (e) => {
  console.log('listener A — capture: false', e.eventPhase);  // → 2
});

btn.addEventListener('click', (e) => {
  console.log('listener B — capture: true', e.eventPhase);   // → 2
}, true);

// Resultado al hacer clic en btn:
// listener A — capture: false 2
// listener B — capture: true  2
```

## Particularidad: ambas fases coexisten

En la fase target, el motor ejecuta todos los listeners del elemento — los registrados con `capture: true` y los registrados con `capture: false` — en orden de registro. No hay separación de fases en el `target` mismo; la distinción captura/burbujeo solo es relevante en los nodos ancestros.

```
listener capture:true registrado 1º  → se ejecuta 1º
listener capture:false registrado 2º → se ejecuta 2º
```

Si se invierten los registros, el orden de ejecución también se invierte.

## `e.eventPhase`

La propiedad `e.eventPhase` informa de la fase activa en el momento en que el listener se ejecuta:

| Valor | Constante | Fase |
|---|---|---|
| 1 | `Event.CAPTURING_PHASE` | Captura |
| 2 | `Event.AT_TARGET` | Target |
| 3 | `Event.BUBBLING_PHASE` | Burbujeo |

```js
document.addEventListener('click', (e) => {
  // cuando el usuario clica directamente sobre document (raro)
  // → e.eventPhase podría ser 2 si document es el target
});
```

## `e.target` es siempre el mismo

A lo largo de todas las fases, `e.target` apunta al elemento que inició el evento. Cambia entre fases es `e.currentTarget` (el elemento con el listener activo), no `e.target`.

> [!tip]
> Si se necesita que un listener se ejecute solo cuando el evento ocurre exactamente en el elemento (no cuando burbujea desde un hijo), se puede comprobar `e.target === e.currentTarget` al inicio del handler.

```js
contenedor.addEventListener('click', (e) => {
  if (e.target !== e.currentTarget) return;  // ignorar clics en hijos
  console.log('clic directo en el contenedor');
});
```

## Notas relacionadas

- [[01 Captura|Captura]]
- [[03 Burbujeo|Burbujeo]]
- [[04 Fases del Evento/index|Fases del Evento]]
- [[01 target vs currentTarget|target vs currentTarget]]
