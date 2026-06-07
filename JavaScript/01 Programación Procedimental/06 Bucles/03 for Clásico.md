---
title: for Clásico — Bucle con índice explícito
aliases:
  - for loop
  - bucle for
  - for clásico
tags:
  - javascript
  - api/concepto
  - control-flujo
objeto: global
tipo: concepto
draft: false
---

# for Clásico

> [!definicion]
> El `for` clásico agrupa en una sola línea las tres partes del ciclo de control: `for (init; condición; actualización) { cuerpo }`. `init` se ejecuta una sola vez al inicio; `condición` se evalúa antes de cada iteración; `actualización` se ejecuta al final de cada iteración. Las tres partes son opcionales.

```js
for (let i = 0; i < 3; i++) {
  console.log(i); // 0, 1, 2
}

// Iteración inversa
const arr = ["a", "b", "c"];
for (let i = arr.length - 1; i >= 0; i--) {
  console.log(arr[i]); // "c", "b", "a"
}
```

## Cómo funciona por dentro

El motor ejecuta `init` una única vez antes de entrar al bucle. En cada ciclo: evalúa `condición` con ToBoolean → si `false`, sale; si `true`, ejecuta el cuerpo → ejecuta `actualización` → repite la evaluación de `condición`. La variable declarada con `let` en `init` tiene **block scope** acotado al bucle.

```
init
┌──→ condición == false → salir
│    condición == true
│      cuerpo
│      actualización
└────────────────────────┘
```

## Las tres partes son opcionales

Cualquiera de las tres partes puede omitirse. `for (;;)` es un bucle infinito equivalente a `while (true)`.

```js
// init vacío — i ya definida
let i = 0;
for (; i < 5; i++) { /* ... */ }

// actualización vacía — se actualiza dentro del cuerpo
for (let i = 0; i < 5; ) {
  console.log(i);
  i += 2; // saltar de dos en dos
}

// Bucle infinito con break explícito
for (;;) {
  const input = leerInput();
  if (input === "q") break;
  procesar(input);
}
```

## Múltiples variables en `init` y `actualización`

```js
// Convergencia desde los extremos
for (let i = 0, j = 10; i < j; i++, j--) {
  console.log(i, j); // 0 10 → 1 9 → 2 8 → 3 7 → 4 6
}
```

La coma en `actualización` es el **operador coma**: evalúa ambas expresiones de izquierda a derecha. En `init`, la coma separa declaraciones dentro del mismo `let`/`var`.

## Recetas comunes

**Iterar un array con índice**

```js
const frutas = ["manzana", "pera", "uva"];
for (let i = 0; i < frutas.length; i++) {
  console.log(`${i}: ${frutas[i]}`);
}
// 0: manzana
// 1: pera
// 2: uva
```

**Eliminar elementos de un array durante la iteración (iterar al revés)**

```js
const nums = [1, 2, 3, 4, 5];
for (let i = nums.length - 1; i >= 0; i--) {
  if (nums[i] % 2 === 0) nums.splice(i, 1);
}
console.log(nums); // [1, 3, 5]
```

Iterar al revés evita el problema de índices desplazados al eliminar elementos.

**Saltar elementos con paso**

```js
// Procesar solo los elementos en posiciones pares
for (let i = 0; i < arr.length; i += 2) {
  procesar(arr[i]);
}
```

**Llenar una matriz bidimensional**

```js
const matrix = [];
for (let i = 0; i < 3; i++) {
  matrix[i] = [];
  for (let j = 0; j < 3; j++) {
    matrix[i][j] = i * 3 + j;
  }
}
// [[0,1,2],[3,4,5],[6,7,8]]
```

## `for` clásico vs `for...of`

| Caso | Usar |
|---|---|
| Necesito el índice | `for` clásico |
| Iterar al revés | `for` clásico |
| Paso distinto de 1 | `for` clásico |
| Solo valores, sin índice | `for...of` |
| Strings con emojis/Unicode | `for...of` |
| Map, Set, generadores | `for...of` |

Cuando solo se necesitan los valores de un array, [[05 for...of]] es más legible y elimina la posibilidad de errores de off-by-one. Si se necesita índice y valor, `array.entries()` con `for...of` es la alternativa moderna:

```js
for (const [i, v] of frutas.entries()) {
  console.log(`${i}: ${v}`);
}
```

> [!tip] Buenas prácticas
> - Declarar el iterador con `let` (no `var`) para confinarlo al scope del bucle.
> - Calcular `arr.length` fuera del bucle solo si es costoso — los motores modernos optimizan el acceso a `.length` de arrays nativos.
> - Para arrays, preferir `for...of` salvo que el índice sea necesario.

> [!warning] Errores comunes
> **Off-by-one** — confundir `<` con `<=` en la condición al iterar por índice es el error más frecuente:
> ```js
> for (let i = 0; i <= arr.length; i++) { // accede a arr[arr.length] → undefined
>   console.log(arr[i]);
> }
> ```
> **`var` en lugar de `let`** — con `var` el iterador escapa al scope del bucle, lo que causa bugs clásicos en callbacks asíncronos dentro del cuerpo (todos capturan el mismo `i` final). Usar `let` resuelve esto por block scope.

## Notas relacionadas

- [[06 Bucles/index | Bucles]]
- [[01 while]]
- [[05 for...of]]
- [[07 break y continue]]
- [[08 Labeled Statements]]
