---
title: Bucles — Iteración y protocolo iterador
aliases:
  - loops
  - iteración
  - bucles JS
tags:
  - javascript
  - api/concepto
  - control-flujo
objeto: global
tipo: concepto
draft: false
---

# Bucles

> [!definicion]
> Un **bucle** es una estructura de control que repite un bloque de código mientras una condición sea verdadera o hasta que se hayan recorrido todos los elementos de una colección. JavaScript ofrece seis construcciones de bucle: `while`, `do...while`, `for` clásico, `for...in`, `for...of` y `for await...of`, cada una optimizada para un patrón de iteración distinto.

## El protocolo iterador

`for...of` y `for await...of` no operan sobre la longitud de un array — operan sobre el **protocolo iterador** definido en ES6. Un objeto es **iterable** si expone el método `Symbol.iterator` que devuelve un **iterador**: un objeto con método `next()` que retorna `{ value, done }`.

```js
const arr = [10, 20, 30];
const iter = arr[Symbol.iterator]();

iter.next(); // { value: 10, done: false }
iter.next(); // { value: 20, done: false }
iter.next(); // { value: 30, done: false }
iter.next(); // { value: undefined, done: true }
```

Cualquier objeto que implemente `Symbol.iterator` puede usarse con `for...of`: arrays, strings, Map, Set, NodeList, generadores, y objetos personalizados.

El protocolo asíncrono funciona igual pero usando `Symbol.asyncIterator` y devolviendo Promises desde `next()`.

## Tabla comparativa

| Bucle | Evalúa condición | Garantía de ejecución | Itera sobre | Uso típico |
|---|---|---|---|---|
| `while` | antes | ninguna | condición booleana | cantidad desconocida de iteraciones |
| `do...while` | después | al menos 1 vez | condición booleana | init o prompt obligatorio |
| `for` clásico | antes | ninguna | índice numérico | arrays con índice, iteración inversa |
| `for...in` | — | — | propiedades enumerables | inspección de objetos |
| `for...of` | — | — | valores de iterables | arrays, strings, Map, Set |
| `for await...of` | — | — | iterables asíncronos | streams, APIs paginadas |

## Cuándo usar cada uno

`while` — cuando el número de iteraciones depende de un estado que cambia dentro del cuerpo (parsers, reintentos, event loops simulados).

`do...while` — cuando la primera iteración siempre debe ocurrir independientemente de la condición (menús interactivos, validación inicial).

`for` clásico — cuando se necesita el índice, se itera al revés, o se saltan elementos con paso distinto de 1.

`for...in` — solo para inspeccionar propiedades enumerables de un objeto; evitar en arrays.

`for...of` — el reemplazo moderno de `for` clásico cuando no se necesita el índice; correcto para strings Unicode, Map y Set.

`for await...of` — para consumir streams o iterables asíncronos en secuencia dentro de una función `async`.

## Control de flujo dentro de bucles

`break` sale del bucle actual; `continue` salta al inicio de la siguiente iteración. Las **labeled statements** permiten salir de bucles anidados desde el interior. Ver [[07 break y continue]] y [[08 Labeled Statements]].

## Notas relacionadas

- [[01 while]]
- [[02 do...while]]
- [[03 for Clásico]]
- [[04 for...in]]
- [[05 for...of]]
- [[06 for await...of]]
- [[07 break y continue]]
- [[08 Labeled Statements]]
- [[JavaScript/01 Programación Procedimental/05 Condicionales/index | Condicionales]]
- [[JavaScript/07 Programación Asíncrona/06 Async / Await/05 Bucles Asíncronos (for await...of) | Bucles Asíncronos (for await...of)]]
