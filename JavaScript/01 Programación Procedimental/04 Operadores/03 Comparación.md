---
title: Operadores de Comparación — ===, ==, relacionales, Object.is
aliases:
  - comparación JS
  - comparison operators
  - igualdad estricta
  - igualdad abstracta
tags:
  - javascript
  - api/operador
  - operadores
objeto: global
tipo: operador
retorna: boolean
muta: false
asincrono: false
draft: false
---

# Operadores de Comparación

> [!definicion] Comparación
> Los operadores de comparación evalúan una relación entre dos operandos y devuelven `true` o `false`. JS tiene dos tipos de igualdad (estricta y abstracta) y cuatro operadores relacionales. La diferencia crucial es si aplican coerción de tipos antes de comparar.

```js
// Igualdad estricta (preferida siempre)
5 === 5       // → true
5 === "5"     // → false  (tipos distintos, sin coerción)

// Igualdad abstracta (con coerción)
5 == "5"      // → true   (convierte "5" a 5)
null == undefined // → true  (caso especial de la spec)
```

## Igualdad estricta: `===` y `!==`

Devuelve `true` si los operandos tienen el mismo tipo **y** el mismo valor. Sin coerción. Para objetos y arrays, compara por referencia en memoria, no por contenido.

```js
// Primitivos
1 === 1         // → true
1 === "1"       // → false
true === 1      // → false
null === null   // → true
undefined === undefined // → true
null === undefined      // → false

// Objetos: referencia, no contenido
const a = [1, 2, 3];
const b = [1, 2, 3];
a === b  // → false  (distintos objetos en memoria)
const c = a;
a === c  // → true   (misma referencia)

// NaN: caso único — nunca es igual a sí mismo
NaN === NaN  // → false  (usar Number.isNaN para detectarlo)
```

## Igualdad abstracta: `==` y `!=`

Aplica el algoritmo de coerción de ECMAScript (Abstract Equality Comparison) antes de comparar. Los casos más llamativos:

| Comparación | Resultado | Razón |
|-------------|-----------|-------|
| `null == undefined` | `true` | caso especial de la spec |
| `null == 0` | `false` | null solo es igual a undefined |
| `0 == false` | `true` | false → 0 |
| `"" == false` | `true` | ambos → 0 |
| `"0" == false` | `true` | "0"→0, false→0 |
| `[] == false` | `true` | []→""→0, false→0 |
| `[] == ![]` | `true` | ![] → false, [] → 0, false → 0 |
| `"1" == 1` | `true` | "1" → 1 |
| `NaN == NaN` | `false` | NaN nunca es igual |

```js
null == undefined  // → true
null == 0          // → false
0 == false         // → true
"" == false        // → true
[] == ![]          // → true  (el ejemplo más contraintuitivo)
```

> [!warning] Por qué evitar `==`
> El algoritmo de coerción tiene 12 pasos con casos especiales. El comportamiento de `==` es difícil de predecir sin conocer la spec en detalle. En código de producción, usar siempre `===` y convertir explícitamente los tipos antes si es necesario.

## Operadores relacionales: `<`, `>`, `<=`, `>=`

Para números, comparación numérica estándar. Para strings, comparación **lexicográfica** por code point Unicode. Si los operandos son de tipos distintos, se aplica coerción a número.

```js
// Números
5 > 3    // → true
5 <= 5   // → true

// Strings: lexicográfico por Unicode code point
"banana" < "cherry"  // → true   ('b' < 'c')
"10" < "9"           // → true   ('1' < '9', comparación de caracteres)
"Z" < "a"            // → true   (Z=90, a=97 en Unicode)

// Coerción mixta
"5" > 3   // → true   ("5" → 5)
null > 0  // → false
null == 0 // → false
null >= 0 // → true   (null → 0 en relacionales pero no en ==)
```

> [!warning] Ordenar arrays de strings con números
> `["10", "9", "2"].sort()` devuelve `["10", "2", "9"]` porque la comparación es lexicográfica. Para ordenar números correctamente:
> ```js
> [10, 9, 2].sort((a, b) => a - b)  // → [2, 9, 10]
> ```

## NaN no es comparable

`NaN` devuelve `false` en toda comparación, incluyendo la igualdad consigo mismo.

```js
NaN < 1    // → false
NaN > 1    // → false
NaN === NaN // → false
NaN <= NaN  // → false

// Detección correcta
Number.isNaN(NaN)      // → true  (no convierte tipos)
isNaN("texto")         // → true  (convierte antes — comportamiento engañoso)
Number.isNaN("texto")  // → false (correcto)
```

## `Object.is(a, b)` — Igualdad SameValue

Implementa el algoritmo SameValue de la spec, que difiere de `===` en dos casos: `NaN` y `±0`.

| Comparación | `===` | `Object.is` |
|-------------|-------|-------------|
| `NaN, NaN` | `false` | `true` |
| `+0, -0` | `true` | `false` |
| todo lo demás | igual | igual |

```js
Object.is(NaN, NaN)  // → true
Object.is(+0, -0)    // → false
Object.is(1, 1)      // → true
Object.is(1, "1")    // → false
```

`Object.is` es útil cuando se necesita distinguir `+0` de `-0` (relevante en cálculos numéricos o implementación de estructuras de datos) o para comprobar `NaN` de forma directa.

## Recetas comunes

```js
// Ordenar números en array
arr.sort((a, b) => a - b);  // ascendente
arr.sort((a, b) => b - a);  // descendente

// Detectar NaN de forma segura
Number.isNaN(x)

// Comparar con null/undefined de forma segura (acepta ambos)
x == null  // true si x es null O undefined (uso deliberado de ==)

// Comparar objetos por contenido (deep equal)
JSON.stringify(a) === JSON.stringify(b)  // simple, sin ciclos
// Para casos complejos, usar structuredClone o librerías como lodash.isEqual

// Comparar strings ignorando mayúsculas
a.toLowerCase() === b.toLowerCase()
a.localeCompare(b, undefined, { sensitivity: "base" }) === 0
```

## Cómo funciona por dentro

`===` implementa el algoritmo Strict Equality Comparison (§7.2.15 de la spec): primero comprueba tipos; si difieren, `false`; si ambos son `Number`, aplica IEEE 754 equality (donde `NaN ≠ NaN` y `+0 = -0`). Para objetos, compara las referencias de memoria.

`==` implementa Abstract Equality Comparison (§7.2.14): un árbol de 12 reglas que describe qué conversiones aplicar según los tipos de los operandos. Casos especiales codificados directamente: `null == undefined` es `true` sin coerción.

> [!tip] Regla práctica
> Usar `===` siempre. El único uso deliberado aceptable de `==` es `x == null` como forma concisa de comprobar que `x` es `null` o `undefined` (ambos a la vez). Documentarlo con un comentario.

## Notas relacionadas

- [[02 Aritméticos]] — `NaN` generado por operaciones aritméticas
- [[04 Lógicos]] — combinar resultados booleanos con `&&`, `||`
- [[07 Nullish Coalescing (??)]] — diferencia entre nullish (`null`/`undefined`) y falsy
