---
title: Estados de una Promise — pending, fulfilled, rejected
aliases:
  - estados promise
  - promise state
  - PromiseState
tags:
  - javascript
  - api/concepto
  - asincrono
objeto: Promise
tipo: concepto
draft: false
---

# Estados de una Promise

> [!definicion]
> Una Promise puede estar en exactamente uno de tres estados mutuamente excluyentes: **`pending`** (operación en curso), **`fulfilled`** (completada con éxito) o **`rejected`** (fallida). Una vez que pasa de `pending` a cualquiera de los otros dos, el estado es **inmutable** — no puede cambiar nunca. Se dice que la Promise está **settled** (asentada).

```js
const p1 = new Promise(resolve => setTimeout(() => resolve(42), 1000));
// p1: [[PromiseState]]: "pending"   al crearla

// tras 1 segundo:
// p1: [[PromiseState]]: "fulfilled"
//     [[PromiseResult]]: 42

const p2 = Promise.reject(new Error('fallo'));
// p2: [[PromiseState]]: "rejected"
//     [[PromiseResult]]: Error: fallo
```

## Tabla de estados

| Estado | Descripción | Cómo se alcanza | Valor asociado |
|---|---|---|---|
| `pending` | Operación en curso; no se ha producido ningún resultado | Estado inicial al ejecutar `new Promise(executor)` | `undefined` |
| `fulfilled` | Operación terminada con éxito | El executor llama `resolve(valor)` | El valor pasado a `resolve` |
| `rejected` | Operación fallida | El executor llama `reject(razón)` o lanza una excepción | El error o razón pasado a `reject` |

## Flujo de transición

```
new Promise(executor)
        │
        ▼
    [pending]
    /        \
resolve()   reject() / throw
   │              │
[fulfilled]  [rejected]
   │              │
settled ──────────┘   (inmutable a partir de aquí)
```

Una Promise no puede pasar de `fulfilled` a `rejected`, ni volver a `pending`. Cualquier llamada adicional a `resolve` o `reject` una vez settled se ignora silenciosamente.

## Inspección en DevTools (V8)

El motor V8 expone el estado interno mediante dos slots ocultos visibles al imprimir una Promise en la consola del navegador:

- `[[PromiseState]]` — cadena `"pending"`, `"fulfilled"` o `"rejected"`.
- `[[PromiseResult]]` — el valor (si fulfilled) o la razón (si rejected). Es `undefined` mientras la promesa está pending.

```js
const p = fetch('/api/datos'); // devuelve Promise
console.log(p);
// Promise {<pending>}
//   [[Prototype]]: Promise
//   [[PromiseState]]: "pending"
//   [[PromiseResult]]: undefined
```

Estos slots no son accesibles desde código JavaScript — solo son una representación de depuración del motor.

## Cómo funciona por dentro

Cuando se ejecuta `new Promise(executor)`:

1. Se crea el objeto Promise con estado `pending`.
2. El executor se invoca **síncronamente** con dos funciones: `resolve` y `reject`.
3. Si el executor llama a `resolve(valor)`:
   - Si `valor` es una Promise u objeto thenable, la nueva Promise adopta su estado (flattening).
   - Si no, la Promise pasa a `fulfilled` con ese valor.
4. Si el executor llama a `reject(razón)` o lanza una excepción, la Promise pasa a `rejected`.
5. Una vez settled, cualquier handler registrado con `.then`/`.catch` se encola como **microtask** y se ejecuta tras el script actual.

## Settled vs Resolved

La spec de ECMAScript distingue dos términos que se confunden:

- **Settled** — la Promise está en `fulfilled` o `rejected`. Término absoluto: el estado ya no cambia.
- **Resolved** — la Promise tiene un destino fijado, pero ese destino puede ser otra Promise aún `pending`. Una Promise puede estar *resolved* (su destino está fijado) pero aún *pending* (si la Promise a la que apunta no ha terminado).

En la práctica cotidiana "resolved" se usa como sinónimo de "fulfilled", pero la distinción importa al depurar comportamientos de `Promise.resolve(otraPromise)`.

> [!tip]
> Para inspeccionar el estado de una Promise en Node.js desde código: `require('util').inspect(p, { depth: 0 })` muestra `Promise { <pending> }` o `Promise { 42 }` o `Promise { <rejected> Error: ... }`. Útil en tests y scripts de depuración.

> [!warning]
> El estado `pending` no bloquea la ejecución del hilo principal. El código que sigue a la creación de la Promise se ejecuta inmediatamente. El resultado de la operación asíncrona estará disponible solo dentro de un handler `.then` o con `await` — nunca de forma síncrona después de crear la Promise.

## Notas relacionadas

- [[index|Promesas — índice de la sección]]
- [[02 Crear Promesa (resolve, reject)|Crear una Promise — executor, resolve, reject]]
- [[03 then|.then — encadenar sobre fulfilled]]
- [[04 catch|.catch — capturar rejected]]
