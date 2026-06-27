---
title: Array.prototype.reverse — Invertir in-place
aliases:
  - reverse
  - Array.reverse
  - toReversed
tags:
  - javascript
  - api/metodo
  - arrays
objeto: Array
tipo: metodo
retorna: Array
muta: true
asincrono: false
draft: false
order: 11
---

# Array.prototype.reverse

> [!definicion]
> `arr.reverse()` invierte el orden de los elementos del array **in-place** y retorna **el mismo array** (no una copia). El primer elemento pasa a ser el último y viceversa. **Muta** el array original.

## Firma

`arr.reverse()` — sin parámetros. Retorna el mismo array invertido. **Muta.**

## Ejemplo base

```js
const arr = [1, 2, 3, 4, 5];

const resultado = arr.reverse();

console.log(arr);       // → [5, 4, 3, 2, 1]  (mutado)
console.log(resultado); // → [5, 4, 3, 2, 1]  (mismo objeto)
console.log(arr === resultado); // → true  ← misma referencia
```

## La trampa de la referencia compartida

`reverse()` retorna el mismo array, no una copia. Asignarlo a otra variable no protege el original:

```js
const original = [1, 2, 3];
const invertido = original.reverse();

invertido.push(99);
console.log(original); // → [3, 2, 1, 99]  ← también modificado
// `original` e `invertido` son el mismo objeto en memoria
```

## Alternativas inmutables

```js
const arr = [1, 2, 3, 4];

// Pre-ES2023: copia con spread antes de invertir
const inv1 = [...arr].reverse();
console.log(arr);  // → [1, 2, 3, 4]  (sin cambios)
console.log(inv1); // → [4, 3, 2, 1]

// ES2023+: toReversed()
const inv2 = arr.toReversed();
console.log(arr);  // → [1, 2, 3, 4]  (sin cambios)
console.log(inv2); // → [4, 3, 2, 1]
```

`toReversed()` es la alternativa semántica clara — sin necesidad de crear la copia manualmente.

## Recetas comunes

```js
// Mostrar historial en orden inverso
const historial = ["evento1", "evento2", "evento3"];
const reciente = [...historial].reverse();
// → ["evento3", "evento2", "evento1"]

// Invertir string (vía array de caracteres)
const str = "hola";
const invertida = str.split("").reverse().join("");
// → "aloh"

// Ordenar descendente: sort + reverse (menos eficiente que sort con compareFn inversa)
const nums = [3, 1, 4, 1, 5];
[...nums].sort((a, b) => a - b).reverse(); // → [5, 4, 3, 1, 1]
// Preferir: [...nums].sort((a, b) => b - a)
```

> [!tip] Buenas prácticas
> - En código con estado inmutable (React, Redux), usar siempre `[...arr].reverse()` o `arr.toReversed()` (ES2023+).
> - Para ordenar en orden descendente, preferir `arr.sort((a, b) => b - a)` directamente en lugar de `sort` + `reverse` — es una sola pasada.

> [!warning] Errores comunes
> - `const inv = arr.reverse()` — asignar el resultado no crea una copia. `inv` y `arr` son el mismo objeto; cualquier mutación sobre uno afecta al otro.
> - Invertir strings directamente: `"hola".reverse()` lanza `TypeError` — `String` no tiene `reverse`. El idioma correcto es `str.split("").reverse().join("")`.
> - `reverse` en arrays sparse maneja los huecos copiando su posición simétrica sin crear propiedades donde no había: el resultado puede tener huecos en posiciones distintas.

## Cómo funciona por dentro

El motor intercambia los elementos de los extremos hacia el centro: `arr[0] ↔ arr[n-1]`, `arr[1] ↔ arr[n-2]`, etc. Costo O(n/2) = O(n). Operación in-place sin memoria adicional.

## Notas relacionadas

- [[10 sort]]
- [[06 slice]]
- [[index|Arrays]]
