---
title: Conversión Explícita — Number, String y Boolean
aliases:
  - conversión explícita JS
  - explicit type conversion
  - Number() String() Boolean()
tags:
  - javascript
  - api/concepto
  - tipos
objeto: global
tipo: concepto
draft: false
---

# Conversión Explícita (Number, String, Boolean)

> [!definicion]
> La conversión explícita de tipos es la invocada directamente por el programador usando `Number()`, `parseInt()`, `parseFloat()`, `String()` o `Boolean()`. A diferencia de la coerción implícita, la intención es visible en el código y el resultado es predecible.

## `Number(valor)`

Convierte cualquier valor a número aplicando el algoritmo `ToNumber` de la especificación.

| Entrada | Resultado |
|---|---|
| `""` | `0` |
| `"  "` | `0` (espacios se eliminan) |
| `"42"` | `42` |
| `"3.14"` | `3.14` |
| `"0xFF"` | `255` (hex) |
| `"hola"` | `NaN` |
| `"3abc"` | `NaN` (diferencia con parseInt) |
| `true` | `1` |
| `false` | `0` |
| `null` | `0` |
| `undefined` | `NaN` |
| `[]` | `0` (array vacio → `""` → `0`) |
| `[3]` | `3` (array de un elemento → `"3"` → `3`) |
| `[1,2]` | `NaN` (array de varios → `"1,2"` → `NaN`) |
| `{}` | `NaN` (`{}.toString()` → `"[object Object]"`) |

```js
Number("")         // → 0
Number("  ")       // → 0
Number("42")       // → 42
Number("3.14abc")  // → NaN   (diferencia clave con parseFloat)
Number(true)       // → 1
Number(false)      // → 0
Number(null)       // → 0
Number(undefined)  // → NaN
Number([])         // → 0
Number([5])        // → 5
Number([1,2])      // → NaN
```

## `parseInt(string, base)` y `parseFloat(string)`

A diferencia de `Number()`, estas funciones **parsean hasta el primer carácter inválido** y retornan lo que encontraron hasta ese punto.

```js
parseInt("3px")        // → 3       (Number("3px") → NaN)
parseInt("10.9")       // → 10      (descarta la parte decimal)
parseInt("0xFF", 16)   // → 255     (hexadecimal)
parseInt("10", 2)      // → 2       (binario)
parseInt("10", 8)      // → 8       (octal)
parseInt("abc")        // → NaN     (no empieza con dígito)
parseInt("")           // → NaN

parseFloat("3.14px")   // → 3.14
parseFloat("3.14.5")   // → 3.14    (para en el segundo punto)
parseFloat("abc")      // → NaN
```

> [!warning] `parseInt` sin base
> `parseInt` sin el segundo argumento infiere la base según el prefijo. En motores antiguos, `"010"` se interpretaba como octal (→ 8). Siempre pasar la base explícita: `parseInt("10", 10)`.

## `String(valor)`

Convierte cualquier valor a su representación en string. Es **seguro para `null` y `undefined`** a diferencia de `.toString()`:

```js
String(42)          // → "42"
String(3.14)        // → "3.14"
String(true)        // → "true"
String(false)       // → "false"
String(null)        // → "null"     (seguro)
String(undefined)   // → "undefined"  (seguro)
String([1,2,3])     // → "1,2,3"
String({})          // → "[object Object]"
String(Symbol("x")) // → "Symbol(x)"
String(42n)         // → "42"
```

Diferencia con `.toString()`:

```js
null.toString()       // → TypeError: Cannot read properties of null
String(null)          // → "null"  (seguro)
undefined.toString()  // → TypeError
String(undefined)     // → "undefined"  (seguro)
```

`.toString()` también permite especificar base para números:

```js
(255).toString(16)    // → "ff"
(255).toString(2)     // → "11111111"
(255).toString(8)     // → "377"
```

## `Boolean(valor)`

Convierte a `true` o `false` según la tabla falsy. Solo 7 valores son falsy; todo lo demás es truthy.

```js
Boolean(false)       // → false
Boolean(0)           // → false
Boolean(0n)          // → false
Boolean("")          // → false
Boolean(null)        // → false
Boolean(undefined)   // → false
Boolean(NaN)         // → false

Boolean(1)           // → true
Boolean(-1)          // → true
Boolean("0")         // → true   (string "0" no es el número 0)
Boolean("false")     // → true   (string no vacío)
Boolean([])          // → true   (array vacío es truthy)
Boolean({})          // → true   (objeto vacío es truthy)
```

## Doble NOT `!!` como forma idiomática

`!!valor` aplica `!` dos veces: el primero convierte a boolean y lo niega; el segundo lo niega de nuevo, dando el boolean equivalente al valor original.

```js
!!1         // → true
!!0         // → false
!!"texto"   // → true
!!""        // → false
!!null      // → false
!![]        // → true
```

`!!` se usa cuando se quiere el boolean resultado y no el valor original, p. ej. para guardar en una propiedad o retornar desde una función que debe retornar boolean estricto.

## Recetas comunes

```js
// Convertir input de formulario (siempre string) a número
const edad = Number(input.value);
if (Number.isNaN(edad)) { /* input inválido */ }

// Parsear número de CSS con unidad
const px = parseInt(getComputedStyle(el).width, 10);

// Guardar un boolean en estado
const estaActivo = !!elemento.classList.contains("activo");

// Convertir número a string en base 16 con padding
(255).toString(16).padStart(2, "0")   // → "ff"
```

> [!tip] Buenas prácticas
> - Pasar siempre la base a `parseInt`: `parseInt(s, 10)`.
> - Usar `String(v)` en lugar de `v.toString()` cuando el valor puede ser `null` o `undefined`.
> - Usar `Number.isNaN()` para verificar el resultado de `Number()` en lugar de `isNaN()` global.
> - `!!valor` es más conciso que `Boolean(valor)` para conversión en expresiones.

> [!warning] Errores comunes
> - `Number("3px")` → `NaN`; para ese caso usar `parseInt("3px", 10)` → `3`.
> - `Number([])` → `0` y `Number([[3]])` → `3`: comportamiento contraintuitivo de arrays.
> - `+valor` (unario positivo) es equivalente a `Number(valor)` pero menos legible: `+"42"` → `42`.

## Notas relacionadas

- [[02 Conversión Implícita|Conversión Implícita]] — cuándo el motor convierte sin que se pida
- [[03 Truthy y Falsy|Truthy y Falsy]] — los valores que Boolean() convierte a false
- [[01 Primitivos/01 number|number]] — `NaN`, `Number.isNaN()`, `Number.EPSILON`
- [[04 Conversión de Tipos/index|Conversión de Tipos]]
