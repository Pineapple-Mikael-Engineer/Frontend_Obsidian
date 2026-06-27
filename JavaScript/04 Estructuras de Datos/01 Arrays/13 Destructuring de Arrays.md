---
title: Destructuring de Arrays — Asignación por posición desde un iterable
aliases:
  - array destructuring
  - desestructuración de arrays
  - destructuring
tags:
  - javascript
  - api/operador
  - arrays
objeto: Array
tipo: operador
muta: false
asincrono: false
draft: false
order: 13
---

# Destructuring de Arrays

> [!definicion]
> El destructuring de arrays es una sintaxis de asignación que extrae valores de un iterable por posición y los asigna a variables. El motor llama a `[Symbol.iterator]()` en el lado derecho y consume `.next()` tantas veces como haya patrones en el lado izquierdo. No muta el iterable original.

## Sintaxis base

```js
const [a, b, c] = [10, 20, 30];
console.log(a); // → 10
console.log(b); // → 20
console.log(c); // → 30
```

## Omitir posiciones

Las comas sin nombre de variable saltan posiciones del iterable:

```js
const [, segundo, , cuarto] = [1, 2, 3, 4];
console.log(segundo); // → 2
console.log(cuarto);  // → 4
```

## Valores por defecto

Si la posición del iterable retorna `undefined`, se usa el valor por defecto:

```js
const [a = 1, b = 2, c = 3] = [10, 20];
console.log(a); // → 10  (del array)
console.log(b); // → 20  (del array)
console.log(c); // → 3   (por defecto, la posición es undefined)
```

El valor por defecto solo se evalúa si el valor extraído es exactamente `undefined` (no `null`).

## Rest en destructuring

El patrón `...nombre` captura todos los elementos restantes en un nuevo array:

```js
const [primero, segundo, ...resto] = [1, 2, 3, 4, 5];
console.log(primero); // → 1
console.log(segundo); // → 2
console.log(resto);   // → [3, 4, 5]

// rest siempre es un array, aunque no queden elementos
const [x, ...sin] = [42];
console.log(sin); // → []
```

El patrón rest debe ser el **último** elemento del lado izquierdo.

## Intercambio de variables sin temporal

```js
let a = 1, b = 2;
[a, b] = [b, a];
console.log(a); // → 2
console.log(b); // → 1
```

No se necesita variable temporal. El motor evalúa el lado derecho completo antes de asignar.

## Destructuring anidado

```js
const [[x, y], [z, w]] = [[1, 2], [3, 4]];
console.log(x, y, z, w); // → 1 2 3 4

// Combinado con valores por defecto
const [[a = 0, b = 0] = []] = [];
console.log(a, b); // → 0 0
```

## Destructuring en parámetros de función

```js
function head([primero]) {
  return primero;
}
head([10, 20, 30]); // → 10

// Con rest y valores por defecto
function procesar([primera = "sin datos", ...resto] = []) {
  console.log(primera, resto);
}
procesar(["A", "B", "C"]); // → "A" ["B", "C"]
procesar();                // → "sin datos" []
```

## Destructuring en bucles

```js
const pares = [[1, "uno"], [2, "dos"], [3, "tres"]];

for (const [num, texto] of pares) {
  console.log(`${num}: ${texto}`);
}
// → 1: uno
// → 2: dos
// → 3: tres
```

Muy común con `Map.entries()`, `Object.entries()` y `Array.prototype.entries()`.

## Destructuring con cualquier iterable

El lado derecho puede ser cualquier iterable: `string`, `Set`, `Map`, `arguments`, generadores:

```js
// String
const [primera, segunda] = "hola";
console.log(primera, segunda); // → "h" "o"

// Set
const [uno, dos] = new Set([1, 2, 3]);
console.log(uno, dos); // → 1 2

// Generador
function* rango(n) { for (let i = 0; i < n; i++) yield i; }
const [a, b, c] = rango(10);
console.log(a, b, c); // → 0 1 2  (solo consume 3 elementos del generador)
```

## Casos de uso reales

```js
// Retorno múltiple de funciones
function minMax(arr) {
  return [Math.min(...arr), Math.max(...arr)];
}
const [min, max] = minMax([3, 1, 4, 1, 5]);

// Extraer headers y datos de CSV parseado
const [cabeceras, ...filas] = csvLines;

// Promise.all con destructuring
const [usuario, pedidos] = await Promise.all([fetchUser(id), fetchOrders(id)]);

// Regex match
const [, año, mes, dia] = "2026-06-07".match(/(\d{4})-(\d{2})-(\d{2})/);
```

## Cómo funciona por dentro

El motor ejecuta `const iter = rhs[Symbol.iterator]()` en el lado derecho. Por cada variable en el patrón izquierdo, llama a `iter.next()` y usa el `value`. Si la llamada retorna `{ done: true }`, las variables restantes reciben `undefined` (o sus valores por defecto). El patrón `...rest` llama `.next()` hasta que `done` sea `true` y acumula los valores en un array.

> [!tip] Buenas prácticas
> - Usar destructuring en parámetros de función cuando solo se necesitan algunos elementos de un array — hace explícita la estructura esperada.
> - Con valores por defecto en destructuring de parámetros, el array completo también puede tener un defecto: `function f([a, b] = [])` evita errores si se llama sin argumento.
> - Para arrays largos donde solo interesa el primer elemento, `const [head] = arr` es más legible que `arr[0]`.

> [!warning] Errores comunes
> - Destruir `null` o `undefined` lanza `TypeError`: `const [a] = null` → `TypeError: null is not iterable`. Asegurar que el lado derecho sea iterable.
> - El patrón rest no puede tener coma al final: `const [a, ...b,] = arr` es un `SyntaxError`.
> - Los valores por defecto solo activan con `undefined`, no con `null`: `const [a = 1] = [null]` → `a = null`.
> - Confundir destructuring de array con destructuring de objeto: `const { 0: primero } = arr` también funciona pero es más verboso.

## Notas relacionadas

- [[01 Creación]]
- [[02 Acceso por Índice y length]]
- [[index|Arrays]]
