---
title: "Métodos de Array Funcionales — Índice"
aliases:
  - Métodos funcionales de array
  - Array methods funcionales
tags:
  - javascript
  - api/metodo
  - arrays
draft: false
order: 4
---

# Métodos de Array Funcionales

Los métodos funcionales de array son funciones de orden superior integradas en el prototipo de `Array`. Todos reciben un **callback** con la firma `(elemento, índice, array)` y operan sin mutar el array original (excepto que el propio callback lo haga explícitamente).

> [!definicion]
> Un método funcional de array es aquel que aplica una función (callback) a los elementos del array para producir un resultado: un nuevo array, un valor único, un booleano, o ninguno. Todos siguen el patrón `array.método(callback(elem, i, arr), [thisArg])`.

El patrón común del callback es:

```js
array.método((elemento, índice, arrayCompleto) => {
  // elemento   → valor actual de la iteración
  // índice     → posición (0-based)
  // arrayCompleto → referencia al array original
});
```

## Clasificación de los 12 métodos

| Método | Acción | Devuelve | Muta el original |
|---|---|---|---|
| `forEach` | Ejecuta un efecto por cada elemento | `undefined` | No |
| `map` | Transforma cada elemento | Nuevo array (mismo tamaño) | No |
| `flatMap` | Transforma y aplana un nivel | Nuevo array (tamaño variable) | No |
| `filter` | Selecciona elementos que cumplen una condición | Nuevo array (igual o menor tamaño) | No |
| `reduce` | Acumula un único valor de izquierda a derecha | Cualquier tipo | No |
| `reduceRight` | Acumula un único valor de derecha a izquierda | Cualquier tipo | No |
| `find` | Busca el primer elemento que cumple la condición | El elemento o `undefined` | No |
| `findIndex` | Busca el índice del primer elemento que cumple | Número (`-1` si no encuentra) | No |
| `findLast` | Busca el último elemento que cumple la condición | El elemento o `undefined` | No |
| `findLastIndex` | Busca el índice del último elemento que cumple | Número (`-1` si no encuentra) | No |
| `some` | Comprueba si al menos un elemento cumple | `boolean` | No |
| `every` | Comprueba si todos los elementos cumplen | `boolean` | No |
| `flat` | Aplana arrays anidados | Nuevo array aplanado | No |

## Grupos funcionales

**Transformación:** `map`, `flatMap` — producen un array transformado.

**Filtrado:** `filter` — seleccionan un subconjunto.

**Reducción:** `reduce`, `reduceRight` — colapsan el array a un valor escalar (número, string, objeto, array nuevo, etc.).

**Búsqueda:** `find`, `findIndex`, `findLast`, `findLastIndex` — localizan elementos o sus posiciones.

**Comprobación:** `some`, `every` — responden preguntas de existencia o universalidad.

**Aplanado:** `flat` — eliminan niveles de anidamiento.

## Cuándo usar cada grupo

Cuando necesitas ejecutar efectos secundarios sin producir un valor → `forEach`. Cuando necesitas transformar cada elemento en otro → `map`. Cuando necesitas expandir cada elemento en cero o más → `flatMap`. Cuando necesitas quedarte con un subconjunto → `filter`. Cuando necesitas combinar todos los elementos en un resultado único → `reduce` / `reduceRight`. Cuando necesitas localizar un elemento concreto → `find` / `findIndex` / `findLast` / `findLastIndex`. Cuando necesitas verificar una propiedad sobre el conjunto → `some` / `every`. Cuando necesitas eliminar niveles de anidamiento → `flat`.

## Notas relacionadas

- [[01 forEach]]
- [[02 map]]
- [[03 filter]]
- [[04 reduce]]
- [[05 reduceRight]]
- [[06 find y findIndex]]
- [[07 findLast y findLastIndex]]
- [[08 some]]
- [[09 every]]
- [[10 flat]]
- [[11 flatMap]]
- [[12 Encadenamiento de Métodos]]
