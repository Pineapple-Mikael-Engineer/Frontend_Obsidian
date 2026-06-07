---
title: for...of — Iterar valores de cualquier iterable
aliases:
  - for of
  - for...of loop
tags:
  - javascript
  - api/concepto
  - control-flujo
objeto: global
tipo: concepto
draft: false
---

# for...of

> [!definicion]
> `for...of` itera los **valores** de cualquier objeto **iterable** — cualquier objeto que implemente `Symbol.iterator`. Sintaxis: `for (const valor of iterable) { cuerpo }`. Funciona con arrays, strings, Map, Set, NodeList, generadores y cualquier objeto con `Symbol.iterator` definido.

```js
const frutas = ["manzana", "pera", "uva"];
for (const fruta of frutas) {
  console.log(fruta); // "manzana", "pera", "uva"
}
```

## Cómo funciona por dentro

El motor llama a `iterable[Symbol.iterator]()` para obtener un **iterador**. En cada iteración llama a `iterador.next()`, que retorna `{ value, done }`. Mientras `done` sea `false`, asigna `value` a la variable declarada y ejecuta el cuerpo. Cuando `done` es `true`, sale del bucle.

```js
// Equivalente manual de for...of
const arr = [1, 2, 3];
const iter = arr[Symbol.iterator]();
let resultado = iter.next();
while (!resultado.done) {
  console.log(resultado.value);
  resultado = iter.next();
}
```

## Con arrays

```js
const nums = [10, 20, 30];

// Solo valores
for (const n of nums) {
  console.log(n); // 10, 20, 30
}

// Índice y valor con entries()
for (const [i, n] of nums.entries()) {
  console.log(`${i}: ${n}`); // "0: 10", "1: 20", "2: 30"
}
```

## Con strings — puntos de código Unicode

`for...of` itera **puntos de código Unicode** (code points), no unidades de código UTF-16. Es la forma correcta de iterar strings que contienen emojis o caracteres del plano suplementario.

```js
const texto = "hola";
for (const char of texto) {
  console.log(char); // "h", "o", "l", "a"
}

// Acceso por índice falla con emojis
const emoji = "👋🌍";
console.log(emoji.length);      // 4 (unidades UTF-16, no caracteres)
console.log(emoji[0]);          // carácter incompleto (surrogate pair)

for (const char of emoji) {
  console.log(char);            // "👋", "🌍" — correcto
}
```

## Con Map

`for...of` sobre un Map itera pares `[clave, valor]` en orden de inserción:

```js
const mapa = new Map([
  ["a", 1],
  ["b", 2],
  ["c", 3],
]);

for (const [clave, valor] of mapa) {
  console.log(`${clave} → ${valor}`);
}
// a → 1
// b → 2
// c → 3
```

## Con Set

```js
const conjunto = new Set([1, 2, 2, 3, 3]);
for (const elem of conjunto) {
  console.log(elem); // 1, 2, 3
}
```

## Con generadores

`for...of` es el mecanismo natural para consumir generadores:

```js
function* rango(inicio, fin, paso = 1) {
  for (let i = inicio; i < fin; i += paso) {
    yield i;
  }
}

for (const n of rango(0, 10, 2)) {
  console.log(n); // 0, 2, 4, 6, 8
}
```

## Diferencia clave con `for...in`

| Aspecto | `for...of` | `for...in` |
|---|---|---|
| Produce | valores | claves (strings) |
| Protocolo | `Symbol.iterator` | enumeración de propiedades |
| Incluye heredadas | no | sí |
| Funciona con Map/Set | sí | no |
| Orden garantizado | sí (orden de inserción) | no para claves enteras |

`for...in` opera sobre la **enumeración de propiedades** del prototipo; `for...of` opera sobre el **protocolo iterador**, completamente independiente de propiedades.

## Objetos literales no son iterables por defecto

```js
const obj = { a: 1, b: 2 };
for (const v of obj) { } // TypeError: obj is not iterable
```

Para iterar un objeto con `for...of`, usar `Object.entries()`, `Object.values()` o `Object.keys()`:

```js
for (const [k, v] of Object.entries(obj)) {
  console.log(k, v);
}
```

O implementar `Symbol.iterator` en el objeto.

> [!tip] Buenas prácticas
> - Usar `for...of` como reemplazo por defecto del `for` clásico cuando no se necesita el índice.
> - Para strings con contenido Unicode, siempre preferir `for...of` sobre acceso por índice o `.split("")`.
> - Combinar con `.entries()` cuando se necesita tanto el índice como el valor.

> [!warning] Errores comunes
> **Usarlo en objetos literales** — los objetos planos no son iterables por defecto; lanza `TypeError`. Usar `Object.entries()` primero.
> **Confundirlo con `for...in`** — `for...in` itera claves; `for...of` itera valores. En un array: `for...in` produce `"0"`, `"1"`, `"2"`; `for...of` produce los elementos.
> **`break` dentro de un generador** — al salir del bucle con `break`, el motor llama al método `return()` del iterador si existe, permitiendo limpieza de recursos en generadores.

## Notas relacionadas

- [[06 Bucles/index | Bucles]]
- [[04 for...in]]
- [[03 for Clásico]]
- [[06 for await...of]]
- [[07 break y continue]]
