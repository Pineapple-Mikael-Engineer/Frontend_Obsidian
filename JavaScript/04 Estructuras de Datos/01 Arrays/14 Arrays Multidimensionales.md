---
title: Arrays Multidimensionales — Matrices y arrays de arrays
aliases:
  - matrices JavaScript
  - array de arrays
  - array multidimensional
tags:
  - javascript
  - api/concepto
  - arrays
objeto: Array
tipo: concepto
muta: false
asincrono: false
draft: false
order: 14
---

# Arrays Multidimensionales

> [!definicion]
> JavaScript no tiene un tipo de dato nativo para matrices o arrays N-dimensionales. Se emulan con **arrays de arrays**: cada elemento del array externo es a su vez un array. El acceso se encadena: `matriz[fila][columna]`. Para álgebra lineal seria, los Typed Arrays o librerías especializadas son más eficientes.

## Creación manual

```js
const matriz = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9],
];

matriz[0][0]; // → 1   (fila 0, columna 0)
matriz[1][2]; // → 6   (fila 1, columna 2)
matriz[2][1]; // → 8   (fila 2, columna 1)
```

## Crear matriz NxM con valores iniciales

```js
const N = 3, M = 4;

// BIEN — cada fila es un array distinto
const matriz = Array.from({ length: N }, () => Array(M).fill(0));
// → [[0,0,0,0], [0,0,0,0], [0,0,0,0]]

// MAL — todas las filas comparten el mismo array
const mal = Array(N).fill(Array(M).fill(0));
mal[0][0] = 99;
console.log(mal[1][0]); // → 99  (¡compartido!)
```

El error de `fill` con un array como valor es el antipatrón más común en matrices JS. Cada fila debe ser una referencia distinta.

```js
// Variante con índices disponibles en la factory
const tablero = Array.from({ length: 8 }, (_, fila) =>
  Array.from({ length: 8 }, (_, col) => (fila + col) % 2 === 0 ? "⬜" : "⬛")
);
// Crea un tablero de ajedrez 8x8
```

## Recorrer una matriz

```js
const m = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];

// for...of anidado (legible)
for (const fila of m) {
  for (const elem of fila) {
    process(elem);
  }
}

// forEach anidado
m.forEach((fila, i) => {
  fila.forEach((elem, j) => {
    console.log(`m[${i}][${j}] = ${elem}`);
  });
});

// flatMap para transformar cada elemento
const dobles = m.flatMap(fila => fila.map(x => x * 2));
// → [2, 4, 6, 8, 10, 12, 14, 16, 18]
```

## Aplanar a 1D

```js
const m = [[1, 2], [3, 4], [5, 6]];

m.flat();       // → [1, 2, 3, 4, 5, 6]   (profundidad 1)
m.flat(1);      // → [1, 2, 3, 4, 5, 6]   (explícito)

// Arrays de 3 dimensiones
const cube = [[[1, 2], [3, 4]], [[5, 6], [7, 8]]];
cube.flat(2);   // → [1, 2, 3, 4, 5, 6, 7, 8]
cube.flat(Infinity); // aplana sin importar la profundidad
```

## Casos de uso reales

```js
// Tablero de juego (tic-tac-toe)
const tablero = Array.from({ length: 3 }, () => Array(3).fill(null));

function marcar(tablero, fila, col, jugador) {
  tablero[fila][col] = jugador;
}

// Tabla de datos (filas × columnas)
const tabla = [
  ["Nombre", "Edad", "Ciudad"],
  ["Ana",    25,     "Madrid"],
  ["Luis",   28,     "Barcelona"],
];

// Transponer una matriz
function transponer(m) {
  return m[0].map((_, j) => m.map(fila => fila[j]));
}
transponer([[1, 2, 3], [4, 5, 6]]);
// → [[1, 4], [2, 5], [3, 6]]

// Multiplicación de matrices (simple, O(n³))
function multiplicar(A, B) {
  const filas = A.length, cols = B[0].length, k = B.length;
  return Array.from({ length: filas }, (_, i) =>
    Array.from({ length: cols }, (_, j) =>
      Array.from({ length: k }, (_, l) => A[i][l] * B[l][j])
        .reduce((s, x) => s + x, 0)
    )
  );
}
```

## Typed Arrays para matrices de alto rendimiento

Para cálculo numérico intensivo, las matrices emuladas con `Array[][]` tienen overhead de GC y boxing. La alternativa eficiente es usar un `Float64Array` plano con indexación manual:

```js
// Matriz NxM almacenada en buffer plano (row-major)
class Matriz {
  constructor(N, M) {
    this.N = N;
    this.M = M;
    this.data = new Float64Array(N * M);
  }
  get(i, j)      { return this.data[i * this.M + j]; }
  set(i, j, val) { this.data[i * this.M + j] = val; }
}

const m = new Matriz(3, 3);
m.set(0, 0, 1.5);
m.get(0, 0); // → 1.5
```

Para álgebra lineal de producción (SVD, inversión, sistemas lineales), considerar librerías como `ml-matrix` o `numeric.js`.

> [!tip] Buenas prácticas
> - Siempre crear filas con `Array.from({ length: N }, () => [...])` o un bucle — nunca con `fill(array)`.
> - Documentar explícitamente el orden de los índices (fila-columna vs columna-fila).
> - Para matrices densas con operaciones numéricas frecuentes, evaluar si `Float64Array` plano supera el overhead de la indexación manual.

> [!warning] Errores comunes
> - `Array(N).fill(Array(M).fill(0))` — todas las filas comparten la misma referencia. Modificar una fila modifica todas.
> - Confundir el orden de índices: en convención matemática `m[i][j]` es fila `i`, columna `j`. Mantenerlo consistente.
> - `matrix.flat()` aplana solo 1 nivel por defecto — para matrices de 3+ dimensiones, pasar la profundidad explícita.
> - Acceder a `matriz[fila][col]` cuando `fila` está fuera de rango lanza `TypeError: Cannot read properties of undefined` (porque `matriz[fila]` es `undefined`). Verificar límites antes de acceder.

## Notas relacionadas

- [[01 Creación]]
- [[12 fill y copyWithin]]
- [[13 Destructuring de Arrays]]
- [[index|Arrays]]
