---
title: Primitivos — Los 7 tipos primitivos de JavaScript
aliases:
  - primitivos JS
  - primitive types
tags:
  - javascript
  - api/concepto
  - tipos
objeto: global
tipo: concepto
draft: false
---

# Primitivos

> [!definicion]
> Un **primitivo** es un valor inmutable que no es un objeto y no tiene métodos propios. JavaScript tiene 7 tipos primitivos: `number`, `string`, `boolean`, `undefined`, `null`, `symbol` y `bigint`. Se pasan **por valor**: asignar un primitivo copia el valor; no se comparte una referencia.

```js
let x = 10;
let y = x;   // copia del valor
y = 99;
console.log(x); // → 10  (x no se ve afectada)
```

## Los 7 primitivos

| Tipo | `typeof` | Valores representativos | Nota |
|---|---|---|---|
| `number` | `"number"` | `0`, `3.14`, `NaN`, `Infinity` | IEEE 754 de 64 bits |
| `string` | `"string"` | `"hola"`, `'hola'`, `` `hola` `` | Secuencia UTF-16 inmutable |
| `boolean` | `"boolean"` | `true`, `false` | Solo dos valores |
| `undefined` | `"undefined"` | `undefined` | Variable sin inicializar |
| `null` | `"object"` | `null` | Bug historico de `typeof` |
| `symbol` | `"symbol"` | `Symbol("desc")` | Unico e irrepetible |
| `bigint` | `"bigint"` | `9007199254740991n` | Entero de precision arbitraria |

## Inmutabilidad

Un primitivo no puede cambiar "en su lugar". Las operaciones sobre primitivos producen valores nuevos; el original permanece intacto.

```js
let s = "mundo";
let s2 = s.toUpperCase(); // produce "MUNDO", s intacto
console.log(s);           // → "mundo"

let n = 5;
let n2 = n + 1;           // produce 6; n no muta
console.log(n);           // → 5
```

## Auto-boxing

Los primitivos `number`, `string` y `boolean` tienen **objetos wrapper** (`Number`, `String`, `Boolean`). Cuando se accede a una propiedad o método sobre el primitivo, el motor lo envuelve temporalmente en el wrapper, ejecuta el acceso, y descarta el wrapper. Esto explica por qué `"hola".toUpperCase()` funciona aunque el string sea un primitivo, no un objeto.

```js
"hola".toUpperCase();
// El motor ejecuta internamente algo equivalente a:
// new String("hola").toUpperCase()  → "HOLA"
// El wrapper se descarta; el primitivo "hola" no cambia.
```

> [!warning] No confundir primitivo con wrapper
> `typeof "hola"` → `"string"` (primitivo). `typeof new String("hola")` → `"object"` (wrapper). El wrapper raramente se usa directamente; genera comportamientos inesperados en comparaciones (`new String("a") !== new String("a")`).

`null` y `undefined` no tienen wrapper y lanzan `TypeError` si se intenta acceder a propiedades sobre ellos.

`symbol` y `bigint` también tienen wrappers (`Symbol`, `BigInt`) pero el motor hace el auto-boxing sin que se suelan usar directamente.

## Paso por valor vs. por referencia

La distinción primitivo/objeto es idéntica a la distinción valor/referencia:

```js
// Primitivo: la función recibe una copia
function incrementar(n) { n += 1; }
let val = 10;
incrementar(val);
console.log(val); // → 10  (no cambió)

// Objeto: la función recibe la misma referencia
function agregar(arr, x) { arr.push(x); }
let lista = [1, 2];
agregar(lista, 3);
console.log(lista); // → [1, 2, 3]  (mutado)
```

## Notas relacionadas

- [[01 number|number]]
- [[02 string|string]]
- [[03 boolean|boolean]]
- [[04 undefined|undefined]]
- [[05 null|null]]
- [[06 symbol|symbol]]
- [[07 bigint|bigint]]
- [[03 Tipos de Datos/index|Tipos de Datos]]
