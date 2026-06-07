---
title: Arrays — Colecciones ordenadas indexadas numéricamente
aliases:
  - Array
  - arrays en JavaScript
tags:
  - javascript
  - api/concepto
  - arrays
objeto: Array
tipo: concepto
muta: false
asincrono: false
draft: false
---

# Arrays

> [!definicion]
> Un Array en JavaScript es un **objeto** cuyas claves son índices numéricos enteros no negativos y que mantiene una propiedad especial `length` igual al índice más alto registrado más uno. No es una estructura de datos primitiva; es un objeto ordinario con comportamiento especial del motor para claves numéricas consecutivas.

Los arrays se representan con la sintaxis literal `[]`. El motor los optimiza internamente como arrays contiguos en memoria (elementos de tipo uniforme en V8 → `PACKED_SMI_ELEMENTS`, `PACKED_DOUBLE_ELEMENTS`, etc.) mientras el patrón de acceso sea predecible.

```js
const nums = [10, 20, 30];
console.log(typeof nums);       // → "object"
console.log(Array.isArray(nums)); // → true
console.log(nums.length);       // → 3
```

## Creación y acceso

Las formas de crear arrays y acceder a sus elementos se desarrollan en las notas [[01 Creación]] y [[02 Acceso por Índice y length]].

## Métodos — clasificación

| N° | Método/Tema | Acción principal | Muta |
|----|-------------|-----------------|------|
| 01 | Creación | `[]`, `Array.of`, `Array.from`, `new Array` | — |
| 02 | Acceso por Índice y `length` | `arr[i]`, `arr.at(i)`, `arr.length` | no |
| 03 | `push` y `pop` | Añadir/eliminar al final | **sí** |
| 04 | `shift` y `unshift` | Eliminar/añadir al inicio | **sí** |
| 05 | `splice` | Eliminar, insertar o reemplazar en posición arbitraria | **sí** |
| 06 | `slice` | Extraer subarreglo sin mutar | no |
| 07 | `concat` | Unir arrays en uno nuevo | no |
| 08 | `includes`, `indexOf`, `lastIndexOf` | Búsqueda por valor | no |
| 09 | `join` | Convertir a string con separador | no |
| 10 | `sort` | Ordenar in-place | **sí** |
| 11 | `reverse` | Invertir in-place | **sí** |
| 12 | `fill` y `copyWithin` | Rellenar / copiar segmento sobre sí mismo | **sí** |
| 13 | Destructuring de Arrays | Asignación por posición desde un iterable | — |
| 14 | Arrays Multidimensionales | Arrays de arrays, matrices | — |

## Métodos mutantes vs inmutables

**Mutan el array original:** `push`, `pop`, `shift`, `unshift`, `splice`, `sort`, `reverse`, `fill`, `copyWithin`.

**Retornan nuevo array o valor, sin mutar:** `slice`, `concat`, `includes`, `indexOf`, `lastIndexOf`, `join`.

**Alternativas inmutables ES2023:** `toSorted()`, `toReversed()`, `toSpliced()`, `with()`.

## Propiedad `length`

`length` no es el número de elementos cuando el array es *sparse*; es el índice más alto + 1. Un array sparse tiene huecos: posiciones que no existen como propiedades en el objeto. Los métodos iterativos (`map`, `filter`, `forEach`) saltan huecos; `join` y `find` los tratan como `undefined`.

## Notas relacionadas

- [[01 Creación]]
- [[02 Acceso por Índice y length]]
- [[03 push y pop]]
- [[04 shift y unshift]]
- [[05 splice]]
- [[06 slice]]
- [[07 concat]]
- [[08 includes, indexOf, lastIndexOf]]
- [[09 join]]
- [[10 sort]]
- [[11 reverse]]
- [[12 fill y copyWithin]]
- [[13 Destructuring de Arrays]]
- [[14 Arrays Multidimensionales]]
