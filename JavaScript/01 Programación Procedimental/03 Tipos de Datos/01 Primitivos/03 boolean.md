---
title: boolean — El tipo lógico de JavaScript
aliases:
  - boolean JS
  - tipo boolean
  - booleano
tags:
  - javascript
  - api/concepto
  - tipos
objeto: global
tipo: concepto
retorna: boolean
muta: false
draft: false
order: 3
---

# boolean

> [!definicion]
> `boolean` es el tipo primitivo con exactamente dos valores: `true` y `false`. Se usa en condiciones, operadores lógicos y como resultado de comparaciones. Cualquier valor de JavaScript se convierte implícitamente a boolean en un contexto booleano (condición de `if`, operando de `&&`/`||`/`!`).

```js
typeof true   // → "boolean"
typeof false  // → "boolean"

1 > 0         // → true
"a" === "b"   // → false
```

## Contextos booleanos

Un valor se evalúa como boolean en:

- `if (expr)`, `while (expr)`, `for (cond)`, el operador ternario `cond ? a : b`.
- Los operadores `&&`, `||`, `!`.
- `Boolean(valor)` o el doble NOT `!!valor`.

## Valores falsy y truthy

Los únicos valores que se convierten a `false` (falsy) son exactamente 6 (más `0n` en ES2020):

| Valor falsy | Tipo |
|---|---|
| `false` | boolean |
| `0` | number |
| `""` | string (vacio) |
| `null` | null |
| `undefined` | undefined |
| `NaN` | number |
| `0n` | bigint (ES2020) |

Todo lo demás es **truthy**: objetos, arrays (incluso vacíos), strings no vacíos, números distintos de cero.

```js
Boolean([])        // → true  (array vacío es truthy)
Boolean({})        // → true  (objeto vacío es truthy)
Boolean("0")       // → true  (string "0" es truthy, no es el número 0)
Boolean("false")   // → true  (string no vacío)
Boolean(0)         // → false
Boolean("")        // → false
```

Los valores truthy/falsy están delegados en detalle en [[04 Conversión de Tipos/03 Truthy y Falsy|Truthy y Falsy]].

## Conversión explícita

```js
Boolean(1)        // → true
Boolean(0)        // → false
Boolean("hola")   // → true
Boolean("")       // → false
Boolean(null)     // → false

// Forma idiomática con doble NOT:
!!1               // → true
!!0               // → false
!!"texto"         // → true
!!null            // → false
```

`!!valor` se usa cuando se quiere guardar el boolean, no el valor original, como resultado.

## Cortocircuito de `&&` y `||`

`&&` y `||` no devuelven necesariamente `true` o `false`; devuelven el **valor que determina el resultado**.

```js
// && devuelve el primer falsy, o el último operando si todos son truthy
"a" && "b"    // → "b"
0 && "b"      // → 0       (0 es falsy → cortocircuito)
null && algo  // → null    (null es falsy → cortocircuito, algo no se evalúa)

// || devuelve el primer truthy, o el último operando si todos son falsy
"a" || "b"    // → "a"
0 || "b"      // → "b"     (0 es falsy → sigue evaluando)
null || ""    // → ""      (ambos falsy → retorna el último)
```

Esto permite escribir guards y valores por defecto:

```js
const nombre = inputUsuario || "Anónimo";   // usa "Anónimo" si inputUsuario es falsy
const log = debug && console.log;           // log solo existe si debug es truthy
```

> [!warning] Trampa de `||` con valores válidos falsy
> `const puerto = opcion || 3000` falla si `opcion` es `0` (un puerto válido): `0` es falsy, por lo que el resultado siempre sería `3000`. Para nullish coalescing (solo `null` o `undefined`), usar `??`: `const puerto = opcion ?? 3000`.

El operador `??` está delegado en [[04 Conversión de Tipos/03 Truthy y Falsy|Truthy y Falsy]].

## El operador `!`

```js
!true       // → false
!false      // → true
!0          // → true
!"texto"    // → false
!null       // → true
```

## Comparaciones que producen boolean

```js
// Igualdad estricta
3 === 3       // → true
3 === "3"     // → false

// Relacionales
5 > 3         // → true
"a" < "b"     // → true  (orden lexicográfico)

// Métodos que retornan boolean
[1,2,3].includes(2)          // → true
"hola".startsWith("ho")      // → true
Number.isNaN(NaN)            // → true
```

> [!tip] Buenas prácticas
> - Usar `!!valor` para convertir a boolean cuando sea necesario guardarlo.
> - En guards donde el valor válido puede ser `0`, `""` o `false`, preferir `?? ` sobre `||`.
> - Comparar con `===` (estricto) en lugar de `==` para evitar coerción de tipos.

## Notas relacionadas

- [[04 Conversión de Tipos/03 Truthy y Falsy|Truthy y Falsy]] — los 6+1 valores falsy y sus implicaciones
- [[04 Conversión de Tipos/04 Igualdad (== vs ===)|Igualdad == vs ===]]
- [[04 Conversión de Tipos/01 Conversión Explícita (Number, String, Boolean)|Conversión Explícita]]
- [[01 Primitivos/index|Primitivos]]
