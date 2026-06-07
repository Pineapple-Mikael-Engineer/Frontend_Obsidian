---
title: Spread Operator — Copias y fusiones inmutables
aliases:
  - spread
  - operador spread
  - rest spread
tags:
  - javascript
  - api/operador
  - funcional
draft: false
---

# Spread Operator `...`

> [!definicion]
> `...iterable` expande un iterable (array, string, Set, etc.) en sus elementos individuales cuando se usa en un literal de array o en una llamada a función. Sobre objetos, `...obj` copia las **propiedades propias enumerables** (sin Symbols de propiedades no enumerables, sin el prototipo) al objeto destino. Produce siempre una nueva estructura sin mutar la original.

```js
// Clonar array
const original = [1, 2, 3];
const copia = [...original];         // [1, 2, 3] — nueva referencia

// Clonar objeto
const user = { id: 1, nombre: "Ana" };
const copia2 = { ...user };          // { id: 1, nombre: "Ana" } — nueva referencia

original === copia;   // false
user === copia2;      // false
```

## Shallow copy: qué significa en la práctica

Spread copia los valores directamente. Para primitivos (números, strings, booleanos), la copia es completa. Para objetos o arrays anidados, se copia la **referencia**, por lo que ambas estructuras comparten el mismo objeto interno.

```js
const estado = { usuario: { nombre: "Ana" }, activo: true };
const nuevo = { ...estado };

nuevo.activo = false;           // solo afecta a nuevo
nuevo.usuario.nombre = "Luis";  // ¡afecta también a estado!

console.log(estado.activo);         // true  — primitivo: copiado por valor
console.log(estado.usuario.nombre); // "Luis" — objeto anidado: referencia compartida
```

Para objetos anidados, `structuredClone(obj)` produce una copia profunda real.

## Recetas de uso inmutable

### Clonar

```js
const arr2 = [...arr];
const obj2 = { ...obj };
```

### Combinar arrays

```js
const a = [1, 2];
const b = [3, 4];
const c = [...a, ...b, 5];  // [1, 2, 3, 4, 5]
```

### Combinar objetos (propiedades duplicadas: la derecha sobreescribe)

```js
const defaults = { color: "azul", tamaño: "M", visible: true };
const overrides = { color: "rojo", visible: false };
const config = { ...defaults, ...overrides };
// { color: "rojo", tamaño: "M", visible: false }
```

### Actualizar una propiedad de objeto

```js
const producto = { id: 1, nombre: "Laptop", precio: 1000 };
const actualizado = { ...producto, precio: 850 };
// { id: 1, nombre: "Laptop", precio: 850 }
```

### Insertar un elemento en una posición

```js
const lista = ["a", "b", "d", "e"];
const i = 2;
const conC = [...lista.slice(0, i), "c", ...lista.slice(i)];
// ["a", "b", "c", "d", "e"]
```

### Reemplazar un elemento por índice

```js
const items = ["x", "y", "z"];
const i = 1;
const nuevo = [...items.slice(0, i), "Y", ...items.slice(i + 1)];
// ["x", "Y", "z"]
```

### Eliminar un elemento por índice

```js
const items = ["a", "b", "c"];
const i = 1;
const sinB = [...items.slice(0, i), ...items.slice(i + 1)];
// ["a", "c"]
```

### Pasar array como argumentos a una función

```js
const nums = [3, 1, 4, 1, 5];
Math.max(...nums);  // 5
```

## Cómo funciona por dentro

**En arrays:** spread usa el protocolo iterador de JavaScript (`Symbol.iterator`). Si el objeto tiene `Symbol.iterator`, spread llama al iterador y consume sus valores en orden. Por eso funciona con strings, Sets, Maps, generadores y cualquier iterable personalizado.

```js
const set = new Set([1, 2, 3, 2, 1]);
[...set];  // [1, 2, 3] — itera el Set en orden de inserción
```

**En objetos:** spread sobre literales de objeto usa el algoritmo `CopyDataProperties` de la especificación ECMAScript. Este itera las claves propias enumerables (las que devolvería `Object.keys()`), incluyendo Symbols enumerables propios, y las copia al objeto destino creando propiedades de datos directamente (sin activar setters). Los Symbols propios enumerables también se copian (igual que `Object.assign`).

```js
const sym = Symbol("key");
const src = { [sym]: 42, normal: "hola" };
const dest = { ...src };
dest[sym]; // 42 — Symbols enumerables propios se copian
```

> [!tip] Buenas prácticas
> - Para actualizar una propiedad de un objeto en un estado de React/Redux, siempre usar `{ ...estado, propiedad: nuevoValor }` para preservar las demás propiedades.
> - Poner los overrides a la derecha del spread para que sobreescriban: `{ ...defaults, ...overrides }`.
> - No usar spread para objetos profundamente anidados donde se necesite copia real: usar `structuredClone()` o Immer.
> - En arrays, `[...arr]` es la forma más idiomática y legible de clonar superficialmente.

> [!warning] Errores comunes
> - **Confundir shallow copy con deep copy:** mutar un objeto anidado en la copia afecta al original. Spread solo protege el primer nivel.
> - **Spread de objetos no-planos activa `Object.keys` interno, no el iterador:** `{ ...miSet }` no expande el Set como array — para eso se usa `[...miSet]`.
> - **Spread en argumento de función con arrays grandes:** si el array tiene cientos de miles de elementos, `fn(...arr)` puede desbordar el tamaño máximo de la pila de llamadas. En ese caso, usar `Function.prototype.apply`.
> - **Sobreescritura no intencional:** `{ ...a, ...b }` donde `b` tiene claves iguales a `a` siempre sobreescribe. Si el orden importa, revisar el orden de los spreads.

## Notas relacionadas

- [[index|Inmutabilidad Práctica — Índice]]
- [[02 Object.assign()]]
- [[04 concat y slice]]
- [[05 toSorted, toReversed, toSpliced]]
