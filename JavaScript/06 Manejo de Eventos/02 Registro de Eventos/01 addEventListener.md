---
title: addEventListener — Registrar listeners en elementos
aliases:
  - addEventListener
  - EventTarget.addEventListener
tags:
  - javascript
  - api/metodo
  - eventos
objeto: EventTarget
tipo: metodo
retorna: undefined
muta: false
asincrono: false
draft: false
order: 1
---

# addEventListener

> [!definicion]
> `elemento.addEventListener(tipo, handler, opcionesOCaptura?)` registra un listener en el elemento para el tipo de evento indicado. El mismo elemento puede tener múltiples listeners para el mismo tipo de evento; todos se ejecutan en orden de registro. Registrar el mismo handler dos veces en el mismo elemento + tipo + fase es idempotente: el segundo registro se ignora.

```js
const btn = document.querySelector('#enviar');

function alHacerClic(e) {
  console.log('clic', e.target);
}

btn.addEventListener('click', alHacerClic);
// → se ejecuta cada vez que se hace clic en btn
```

## Parámetros

| Parámetro | Tipo | Descripción |
|---|---|---|
| `tipo` | string | Nombre del evento sin `on`: `'click'`, `'keydown'`, `'submit'` |
| `handler` | Function \| EventListener | Función que recibe el objeto `Event`, o un objeto con `handleEvent` |
| `opcionesOCaptura` | boolean \| object | `true` = captura; objeto con `capture`, `once`, `passive`, `signal` |

## Handler: función o objeto

El handler puede ser una función ordinaria o un objeto que implemente la interfaz `EventListener` (objeto con método `handleEvent`):

```js
// Función ordinaria
btn.addEventListener('click', function(e) {
  console.log(this);  // → el elemento btn (si es función regular)
});

// Arrow function — this léxico del contexto exterior
btn.addEventListener('click', (e) => {
  console.log(this);  // → el ámbito léxico exterior (p. ej. window)
});

// Objeto con handleEvent — this es el objeto, no el elemento
const handler = {
  contador: 0,
  handleEvent(e) {
    this.contador++;
    console.log('clics:', this.contador);  // this correcto sin bind
  }
};
btn.addEventListener('click', handler);
```

El objeto con `handleEvent` es útil cuando el handler necesita mantener estado propio y `this` correcto sin recurrir a `.bind()`.

## `this` dentro del handler

Para una función regular, `this` es el elemento al que está registrado el listener (equivale a `e.currentTarget`). Para una arrow function, `this` hereda del ámbito léxico del punto de definición, ignorando el elemento.

```js
class Contador {
  constructor(btn) {
    this.n = 0;
    // Arrow: this es la instancia Contador, no el botón
    btn.addEventListener('click', () => this.n++);
  }
}
```

## Registro duplicado

Si se llama `addEventListener` con el mismo tipo, la misma referencia de función y la misma fase (captura o burbujeo), el segundo registro se descarta. Dos funciones distintas con el mismo código son referencias distintas y ambas se registran.

```js
function handler(e) { console.log('clic'); }

btn.addEventListener('click', handler);
btn.addEventListener('click', handler);  // ignorado — misma referencia
// → 'clic' se imprime una sola vez por clic
```

## Cómo funciona por dentro

El motor mantiene una lista interna de listeners por nodo y tipo de evento. Al dispararse un evento, el motor recorre la lista y llama a cada listener en orden de registro, pasando el mismo objeto `Event`. Si un listener lanza una excepción, los siguientes del mismo nodo no se ejecutan.

> [!tip]
> Para poder eliminar un listener con `removeEventListener`, la función debe tener una referencia nombrada. Las arrow functions inline no son eliminables porque no hay forma de pasar la misma referencia al llamar `removeEventListener`.

> [!warning]
> **Pérdida de `this` al extraer un método como callback.** `btn.addEventListener('click', obj.metodo)` pasa la función sin el contexto de `obj`. Usar `.bind(obj)` o una arrow function wrapper: `btn.addEventListener('click', (e) => obj.metodo(e))`.

## Notas relacionadas

- [[02 Opciones (once, passive, capture)|Opciones de addEventListener]]
- [[03 removeEventListener|removeEventListener]]
- [[03 El Objeto Event/index|El Objeto Event]]
- [[04 Fases del Evento/index|Fases del Evento]]
