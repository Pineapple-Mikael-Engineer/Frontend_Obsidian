---
title: Array.from() — Crear arrays desde iterables y array-likes
aliases:
  - Array from
  - Array.from
tags:
  - javascript
  - api/funcion
  - funcional
draft: false
---

# Array.from()

> [!definicion]
> `Array.from(origen, mapFn?, thisArg?)` crea un **nuevo array** a partir de cualquier iterable (objetos con `Symbol.iterator`) o array-like (objetos con propiedad `length` e índices numéricos). Si se proporciona `mapFn`, la aplica a cada elemento durante la creación, equivalente a `Array.from(origen).map(mapFn)` pero en una sola pasada.

```js
// Desde un Set (iterable)
Array.from(new Set([1, 2, 2, 3]));  // [1, 2, 3]

// Desde un string (iterable)
Array.from("hola");  // ["h", "o", "l", "a"]

// Desde un array-like
Array.from({ length: 3, 0: "a", 1: "b", 2: "c" });  // ["a", "b", "c"]
```

## Firma

| Parámetro | Tipo | Descripción |
|---|---|---|
| `origen` | iterable o array-like | Objeto a convertir en array |
| `mapFn` | function (opcional) | Se aplica a cada elemento durante la conversión |
| `thisArg` | any (opcional) | `this` para `mapFn` |

| Aspecto | Detalle |
|---|---|
| Valor de retorno | Nuevo `Array` |
| Muta el origen | No |
| Con `mapFn` | Equivalente a `.from(o).map(fn)` pero en una pasada |

## Tipos de entrada admitidos

**Iterables:** objetos con `Symbol.iterator`. Incluye `Array`, `String`, `Set`, `Map`, `NodeList`, el objeto `arguments`, generadores y cualquier iterable personalizado.

**Array-likes:** objetos con propiedad `length` numérica e índices `0`, `1`, … No necesitan `Symbol.iterator`.

```js
// Set → sin duplicados
Array.from(new Set(["a", "b", "a", "c"]));  // ["a", "b", "c"]

// Map → pares [clave, valor]
const map = new Map([["x", 1], ["y", 2]]);
Array.from(map);  // [["x", 1], ["y", 2]]

// NodeList (array-like del DOM)
Array.from(document.querySelectorAll("li"));  // [li, li, li, ...]

// arguments (array-like)
function suma() {
  return Array.from(arguments).reduce((a, b) => a + b, 0);
}
suma(1, 2, 3);  // 6
```

## Con `mapFn`: inicializar y transformar en una pasada

```js
// Generar rango [0, 1, 2, 3, 4]
Array.from({ length: 5 }, (_, i) => i);  // [0, 1, 2, 3, 4]

// Generar rango [1, 2, 3, 4, 5]
Array.from({ length: 5 }, (_, i) => i + 1);  // [1, 2, 3, 4, 5]

// Cuadrados
Array.from({ length: 5 }, (_, i) => i ** 2);  // [0, 1, 4, 9, 16]

// Transformar Set directamente
Array.from(new Set([1, 4, 9]), Math.sqrt);  // [1, 2, 3]
```

El primer argumento de `mapFn` es el elemento (o `undefined` cuando se usa `{ length: n }`), el segundo es el índice. El patrón `(_, i) => i` usa `_` por convención para indicar que el elemento no se usa.

## Strings Unicode: la ventaja sobre `split`

`split("")` divide por unidad de código UTF-16, lo que rompe caracteres fuera del BMP (emojis, caracteres CJK raros). `Array.from` usa el iterador de strings, que opera sobre puntos de código Unicode completos.

```js
const emoji = "😀👋";

emoji.split("");       // ["😀".charAt(0), "😀".charAt(1), "👋".charAt(0), "👋".charAt(1)]
                       // 4 elementos — cada emoji ocupa 2 unidades UTF-16

Array.from(emoji);     // ["😀", "👋"] — 2 elementos correctos

// Longitud "correcta" de un string con emojis
Array.from(emoji).length;  // 2
emoji.length;              // 4 — unidades UTF-16
```

## Comparación: `Array.from({ length: n }, fn)` vs alternativas

```js
// Alternativa más verbosa
Array(5).fill(0).map((_, i) => i);   // [0, 1, 2, 3, 4] — dos pasadas

// Con Array.from: una sola pasada
Array.from({ length: 5 }, (_, i) => i);  // [0, 1, 2, 3, 4]

// TRAMPA: Array(5).map(...) no funciona — el array está vacío (huecos)
Array(5).map((_, i) => i);  // [empty × 5] — map salta huecos
```

`Array(n)` crea un array con `length: n` pero sin propiedades índice propias (huecos); `map` salta esos huecos y no los procesa. `Array.from` accede por índice numérico al origen, por lo que sí genera valores en cada posición.

## Cómo funciona por dentro

Si el argumento tiene `Symbol.iterator`, `Array.from` invoca el iterador y recorre todos los valores que produce, aplicando `mapFn` si se proporcionó. Si el argumento tiene `length` (y no tiene `Symbol.iterator`), accede secuencialmente a `origen[0]`, `origen[1]`, … hasta `origen[length - 1]`, aplicando `mapFn` a cada uno. El resultado es un array estándar construido en el proceso.

> [!tip] Buenas prácticas
> - Para inicializar arrays con longitud fija y valores derivados del índice, `Array.from({ length: n }, (_, i) => expr)` es el patrón más claro y eficiente.
> - Para convertir un `NodeList` a array y usar métodos como `filter` o `map`, `Array.from` es la forma más explícita; el spread `[...nodeList]` también funciona y es más corto.
> - Usar `Array.from` en lugar de `split("")` siempre que el string pueda contener emojis u otros caracteres fuera del BMP.

> [!warning] Errores comunes
> - **`mapFn` recibe `undefined` como elemento cuando el origen es `{ length: n }`:** es el comportamiento esperado — el valor en cada índice no existe, así que es `undefined`. Usar siempre el índice `i` para generar el valor.
> - **No confundir `Array.from([1,2,3])` con `[...[1,2,3]]`:** ambos clonan el array superficialmente, pero `Array.from` acepta también array-likes que spread no admite directamente en objetos.
> - **`Array.from` no aplana:** convierte el iterable en array de primer nivel sin aplanar estructuras anidadas. Para eso, usar `flat()` o `flatMap()`.

## Notas relacionadas

- [[index|Inmutabilidad Práctica — Índice]]
- [[01 Spread Operator]]
- [[04 concat y slice]]
- [[../04 Métodos de Array Funcionales/02 map|map]]
- [[../04 Métodos de Array Funcionales/10 flat|flat]]
