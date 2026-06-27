---
title: Inmutabilidad Práctica — Índice
aliases:
  - inmutabilidad javascript
  - copias inmutables js
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
order: 5
---

# Inmutabilidad Práctica

Los arrays y objetos en JavaScript son mutables por defecto: asignarlos no los copia, y la mayoría de los métodos clásicos (`sort`, `reverse`, `splice`, `push`) modifican el receptor en su lugar. Trabajar de forma **inmutable** significa producir nuevas versiones en lugar de mutar las existentes, lo que hace el flujo de datos predecible y elimina efectos secundarios inesperados.

> [!definicion]
> Una operación inmutable sobre una estructura de datos produce una **nueva estructura** con los cambios aplicados, dejando la original intacta. En JavaScript no existe inmutabilidad real en el lenguaje base (salvo `Object.freeze`), pero sí existen técnicas nativas para operar sin mutar.

El patrón general: en lugar de `arr.push(x)` → `[...arr, x]`; en lugar de `obj.prop = valor` → `{ ...obj, prop: valor }`.

## Técnicas cubiertas

| Técnica | Opera sobre | Desde | Shallow / Deep |
|---|---|---|---|
| Spread `...` | Arrays y objetos | ES2015 | Shallow |
| `Object.assign()` | Objetos | ES2015 | Shallow |
| `Array.from()` | Iterables / array-likes | ES2015 | Shallow (copia la estructura) |
| `concat` / `slice` | Arrays | ES5 | Shallow |
| `toSorted` / `toReversed` / `toSpliced` / `with` | Arrays | ES2023 | Shallow |

Todas estas técnicas producen **copias superficiales** (shallow): los valores primitivos se copian por valor, pero los objetos o arrays anidados se copian por referencia. Para copias profundas de estructuras arbitrarias, usar `structuredClone()` o librerías como Immer.

## Cuándo usar cada técnica

Cuando necesitas clonar o fusionar arrays u objetos con sintaxis concisa → [[01 Spread Operator|Spread `...`]]. Cuando necesitas fusionar objetos con activación de setters o compatibilidad ES5 → [[02 Object.assign()]]. Cuando necesitas convertir un iterable o array-like en array real, o inicializar arrays con longitud → [[03 Array.from()]]. Cuando necesitas operaciones clásicas pre-ES6 o compatibilidad amplia → [[04 concat y slice]]. Cuando necesitas las versiones inmutables de `sort`, `reverse`, `splice` o reemplazar un elemento puntual → [[05 toSorted, toReversed, toSpliced]].

## Notas relacionadas

- [[01 Spread Operator]]
- [[02 Object.assign()]]
- [[03 Array.from()]]
- [[04 concat y slice]]
- [[05 toSorted, toReversed, toSpliced]]
- [[../06 Currying y Composición/index|Currying y Composición — Índice]]
