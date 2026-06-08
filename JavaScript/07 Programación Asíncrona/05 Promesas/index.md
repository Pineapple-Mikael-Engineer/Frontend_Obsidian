---
title: Promesas — objeto para representar un valor futuro
aliases:
  - Promesas
  - Promise
  - promesas javascript
tags:
  - javascript
  - api/clase
  - asincrono
draft: false
---

# Promesas

> [!definicion]
> Una **Promise** es un objeto que representa el resultado eventual de una operación asíncrona. Encapsula el valor futuro (o el error) en un objeto que se puede inspeccionar, encadenar y pasar como cualquier otro valor. Reemplaza el patrón de callbacks eliminando la inversión de control y el anidamiento profundo, y es el fundamento de `async/await`.

El uso más común: encadenar operaciones asíncronas de forma lineal, con un único punto de manejo de errores.

```js
fetch('/api/usuario/42')
  .then(res => res.json())
  .then(usuario => renderizar(usuario))
  .catch(err => mostrarError(err));
```

## Estados de una Promise

| Estado | Descripción | Cómo se alcanza | Valor asociado |
|---|---|---|---|
| `pending` | Operación en curso | Estado inicial al crear la Promise | `undefined` |
| `fulfilled` | Operación completada con éxito | El executor llama `resolve(valor)` | El valor pasado a `resolve` |
| `rejected` | Operación fallida | El executor llama `reject(razón)` o lanza | El error pasado a `reject` |

Una Promise en `fulfilled` o `rejected` se considera **settled**. El estado es inmutable: no puede revertirse.

## Métodos de instancia

| Método | Qué hace | Retorna |
|---|---|---|
| `.then(onFulfilled, onRejected?)` | Registra handlers para fulfilled y rejected | Nueva `Promise` |
| `.catch(onRejected)` | Azúcar para `.then(undefined, onRejected)` | Nueva `Promise` |
| `.finally(onFinally)` | Se ejecuta siempre al settled, sin recibir valor | Nueva `Promise` |

## Métodos de clase (estáticos)

| Método | Se cumple cuando | Se rechaza cuando | Resultado |
|---|---|---|---|
| `Promise.all(iter)` | Todas se cumplen | Cualquiera se rechaza | Array de valores (orden del iterable) |
| `Promise.allSettled(iter)` | Todas se asientan (nunca rechaza) | Nunca | Array de `{status, value/reason}` |
| `Promise.race(iter)` | La primera en asentarse es fulfilled | La primera en asentarse es rejected | Valor o razón de la primera |
| `Promise.any(iter)` | La primera fulfilled | Todas se rechazan | Valor de la primera fulfilled o `AggregateError` |

## Flujo de ejecución

```
new Promise(executor)
  └─ executor se llama síncronamente
       ├─ resolve(valor) → fulfilled → .then(onFulfilled)
       └─ reject(razón)  → rejected  → .catch(onRejected)
                                         └─ .finally(onFinally) — siempre
```

Los callbacks de `.then`/`.catch`/`.finally` se ejecutan como **microtasks** — después del script actual pero antes de macrotasks (timers, eventos).

## Relación con async/await

`async/await` es azúcar sintáctica sobre Promises: `await expr` pausa la ejecución de la función `async` hasta que la Promise de `expr` se asinete, y devuelve su valor (o lanza su razón). El motor sigue usando la misma cola de microtasks internamente.

## Notas relacionadas

- [[01 Estados (pending, fulfilled, rejected)|Estados de una Promise]]
- [[02 Crear Promesa (resolve, reject)|Crear una Promise]]
- [[03 then|.then — encadenar onFulfilled]]
- [[04 catch|.catch — capturar rechazos]]
- [[05 finally|.finally — ejecutar siempre]]
- [[06 Chaining y Propagación de Errores|Chaining y Propagación de Errores]]
- [[07 Promise.all|Promise.all]]
- [[08 Promise.allSettled|Promise.allSettled]]
- [[09 Promise.race|Promise.race]]
- [[10 Promise.any|Promise.any]]
