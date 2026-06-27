---
title: concat y slice — Operaciones inmutables pre-ES6
aliases:
  - Array.concat
  - Array.slice
  - concat
  - slice
tags:
  - javascript
  - api/metodo
  - funcional
draft: false
order: 4
---

# concat y slice

> [!definicion]
> `array.concat(...valores)` devuelve un **nuevo array** con los elementos del original seguidos de los valores o arrays añadidos. `array.slice(inicio?, fin?)` devuelve un **nuevo array** con los elementos desde `inicio` hasta `fin` (exclusivo). Ninguno muta el array original. Son las formas inmutables canónicas de trabajar con arrays anteriores a ES6.

```js
const a = [1, 2, 3];

// concat — añadir elementos
const b = a.concat([4, 5], 6);  // [1, 2, 3, 4, 5, 6]
console.log(a);  // [1, 2, 3] — no mutado

// slice — extraer subarray
const c = a.slice(1, 3);  // [2, 3]
console.log(a);  // [1, 2, 3] — no mutado
```

---

## concat

### Firma

| Parámetro | Tipo | Descripción |
|---|---|---|
| `...valores` | any (variadic) | Arrays o valores individuales a concatenar |

| Aspecto | Detalle |
|---|---|
| Valor de retorno | Nuevo array |
| Muta el original | No |
| Aplana arrays | Un solo nivel de profundidad |
| Aplana no-arrays | No — se insertan como elemento |

### Comportamiento de aplanado

`concat` aplana **solo un nivel** los argumentos que son Arrays. Los valores que no son Arrays se insertan directamente como elementos, sin importar su tipo.

```js
[1].concat([2, [3, 4]], 5);
// [1, 2, [3, 4], 5]
// [3, 4] no se aplana — ya es un nivel más profundo
```

Se puede controlar el aplanado con `Symbol.isConcatSpreadable`:

```js
const obj = { [Symbol.isConcatSpreadable]: true, 0: "x", 1: "y", length: 2 };
[].concat(obj);  // ["x", "y"] — se trata como array
```

### concat vs spread

```js
const a = [1, 2];
const b = [3, 4];

// Equivalentes para fusionar arrays
a.concat(b);    // [1, 2, 3, 4]
[...a, ...b];   // [1, 2, 3, 4]

// concat es más claro para mezclar arrays y valores individuales
a.concat(b, 5, 6);   // [1, 2, 3, 4, 5, 6]
[...a, ...b, 5, 6];  // [1, 2, 3, 4, 5, 6]
```

---

## slice

### Firma

| Parámetro | Tipo | Descripción |
|---|---|---|
| `inicio` | number (opcional) | Índice desde el que empieza (inclusivo); por defecto 0 |
| `fin` | number (opcional) | Índice donde termina (exclusivo); por defecto `length` |

| Aspecto | Detalle |
|---|---|
| Valor de retorno | Nuevo array |
| Muta el original | No |
| Índices negativos | Cuentan desde el final (`-1` = último elemento) |
| `inicio >= fin` | Devuelve array vacío `[]` |

### Recetas con slice

```js
const arr = ["a", "b", "c", "d", "e"];

// Copia completa (shallow clone)
arr.slice();          // ["a", "b", "c", "d", "e"]
arr.slice(0);         // idem

// Desde el índice 2 hasta el final
arr.slice(2);         // ["c", "d", "e"]

// Últimos 2 elementos
arr.slice(-2);        // ["d", "e"]

// Todo menos el último
arr.slice(0, -1);     // ["a", "b", "c", "d"]

// Primer elemento
arr.slice(0, 1);      // ["a"]

// Eliminar el primer elemento (inmutablemente)
arr.slice(1);         // ["b", "c", "d", "e"]

// Eliminar el último elemento (inmutablemente)
arr.slice(0, -1);     // ["a", "b", "c", "d"]
```

### Eliminar elemento por índice con slice

```js
const arr = ["a", "b", "c", "d"];
const i = 2; // eliminar "c"

const sinC = [...arr.slice(0, i), ...arr.slice(i + 1)];
// ["a", "b", "d"]
```

### Obtener el último elemento sin mutar

```js
const arr = [10, 20, 30];
arr.slice(-1)[0];  // 30 — equivalente a arr[arr.length - 1] o arr.at(-1)
```

## Cómo funcionan por dentro

Ambos métodos crean un nuevo objeto `Array` en el heap y copian las referencias (shallow copy). `concat` procesa cada argumento: si es un Array (o tiene `Symbol.isConcatSpreadable: true`), itera sus elementos y los añade uno a uno; si no es un Array, lo añade directamente. `slice` calcula los índices reales (normalizando negativos y fuera de rango) y copia los elementos dentro del rango al nuevo array.

Ninguno involucra el protocolo iterador (`Symbol.iterator`); operan directamente sobre los índices numéricos del array, por lo que son seguros con arrays sparse (preservan los huecos como `undefined` en el resultado).

> [!tip] Buenas prácticas
> - En código moderno, el spread `[...arr]` y `[...a, ...b]` es más idiomático que `slice()` y `concat`. Usar `concat` y `slice` cuando se necesite compatibilidad con entornos sin transpilación o cuando la intención sea más explícita en el contexto.
> - `slice` sin argumentos es equivalente a `[...arr]` para clonar: ambos producen una copia superficial.
> - Para extraer un rango de elementos, `slice` es más legible que manipular con spread y `filter`.

> [!warning] Errores comunes
> - **`slice` no es `splice`:** `slice` no muta y devuelve el subarray. `splice` muta el original y devuelve los elementos eliminados.
> - **`concat` no aplana más de un nivel:** `[1].concat([[2, 3]])` produce `[1, [2, 3]]`. Para aplanar profundo, usar `flat()`.
> - **Índice fuera de rango en `slice`:** si `inicio` es mayor que `length`, devuelve `[]` sin error; si `fin` es mayor que `length`, actúa como si fuera `length`. No lanza excepciones.
> - **Shallow copy:** los objetos dentro del array no se clonan; la copia comparte las mismas referencias.

## Notas relacionadas

- [[index|Inmutabilidad Práctica — Índice]]
- [[01 Spread Operator]]
- [[05 toSorted, toReversed, toSpliced]]
- [[../04 Métodos de Array Funcionales/10 flat|flat]]
