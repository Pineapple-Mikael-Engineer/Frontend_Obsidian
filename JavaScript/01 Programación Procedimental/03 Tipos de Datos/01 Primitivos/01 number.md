---
title: number — El tipo numérico de JavaScript (IEEE 754)
aliases:
  - number JS
  - tipo number
  - Number
tags:
  - javascript
  - api/concepto
  - tipos
objeto: global
tipo: concepto
retorna: number
muta: false
draft: false
---

# number

> [!definicion]
> `number` es el único tipo numérico de JavaScript para valores enteros y de punto flotante. Internamente usa **IEEE 754 de doble precisión (64 bits)**: 1 bit de signo, 11 bits de exponente y 52 bits de mantisa. Esto implica que todos los números son doubles, lo que genera limitaciones en precisión para enteros grandes y en aritmética decimal.

```js
typeof 42       // → "number"
typeof 3.14     // → "number"
typeof NaN      // → "number"
typeof Infinity // → "number"
```

## Valores especiales

| Valor | Descripción | Cómo surge |
|---|---|---|
| `NaN` | Not a Number; resultado de operacion invalida | `0/0`, `parseInt("abc")` |
| `Infinity` | Infinito positivo | `1/0`, `Number.MAX_VALUE * 2` |
| `-Infinity` | Infinito negativo | `-1/0` |
| `-0` | Cero negativo | `0 * -1`, `-0.5` truncado |

```js
0 / 0            // → NaN
1 / 0            // → Infinity
-1 / 0           // → -Infinity
Math.sqrt(-1)    // → NaN
```

## Enteros seguros

El rango de enteros representables sin pérdida de precisión queda delimitado por:

- `Number.MAX_SAFE_INTEGER` = `2^53 - 1` = `9007199254740991`
- `Number.MIN_SAFE_INTEGER` = `-(2^53 - 1)` = `-9007199254740991`

Fuera de ese rango, operaciones enteras producen resultados incorrectos.

```js
Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2 // → true (pérdida de precisión)
Number.isSafeInteger(9007199254740991)  // → true
Number.isSafeInteger(9007199254740992)  // → false
```

Para enteros más allá de ese rango usar [[07 bigint|bigint]].

## NaN y su trampa

`NaN` es el único valor de JavaScript que **no es igual a sí mismo**. La comprobación `valor === NaN` siempre es `false`.

```js
NaN === NaN       // → false
NaN !== NaN       // → true

// Forma correcta:
Number.isNaN(NaN)         // → true
Number.isNaN("texto")     // → false  (no confundir con isNaN() global)
isNaN("texto")            // → true   (isNaN() global convierte antes: isNaN("texto") → isNaN(NaN))
```

> [!warning] `isNaN()` global vs `Number.isNaN()`
> El `isNaN()` global aplica `ToNumber` antes de comprobar, por lo que `isNaN("hola")` devuelve `true` aunque `"hola"` no sea `NaN`. `Number.isNaN()` no realiza coerción y solo devuelve `true` para el valor exacto `NaN`.

## Métodos estáticos de Number

| Método | Descripción | Ejemplo |
|---|---|---|
| `Number.isNaN(v)` | `true` solo si `v === NaN` | `Number.isNaN(NaN)` → `true` |
| `Number.isFinite(v)` | `true` si es finito y no NaN | `Number.isFinite(Infinity)` → `false` |
| `Number.isInteger(v)` | `true` si es entero | `Number.isInteger(3.0)` → `true` |
| `Number.isSafeInteger(v)` | `true` si esta en rango seguro | `Number.isSafeInteger(2**53)` → `false` |
| `Number.parseInt(str, base)` | Parsea hasta caracter invalido | `Number.parseInt("3px")` → `3` |
| `Number.parseFloat(str)` | Parsea decimal hasta invalido | `Number.parseFloat("3.14abc")` → `3.14` |

## Problema de punto flotante

La aritmética decimal no es exacta en IEEE 754:

```js
0.1 + 0.2           // → 0.30000000000000004
0.1 + 0.2 === 0.3   // → false
```

**Estrategias:**

```js
// toFixed: redondear para mostrar (retorna string)
(0.1 + 0.2).toFixed(2)    // → "0.30"

// Comparar con epsilon
Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON   // → true

// Escalar a enteros para la operación
(0.1 * 10 + 0.2 * 10) / 10   // → 0.3
```

`Number.EPSILON` = `2^-52` ≈ `2.22e-16`, la diferencia mínima entre 1 y el siguiente double.

## Literales numéricos

```js
// Decimal
1000
1_000_000   // separador visual (ES2021), equivale a 1000000

// Hexadecimal (base 16)
0xFF        // → 255
0xFF_FF     // → 65535

// Octal (base 8)
0o77        // → 63

// Binario (base 2)
0b1010      // → 10

// Notación científica
1.5e3       // → 1500
2.5e-4      // → 0.00025
```

## Métodos de instancia relevantes

```js
(3.14159).toFixed(2)       // → "3.14"  (redondea, retorna string)
(1234.5).toLocaleString()  // → "1,234.5"  (según locale)
(255).toString(16)         // → "ff"  (convierte a base indicada)
(0.000025).toExponential(2) // → "2.50e-8"
```

## Cómo funciona por dentro (IEEE 754)

La representación de 64 bits se divide en:

```
bit 63     : signo (0 = positivo, 1 = negativo)
bits 62-52 : exponente (11 bits, con sesgo de 1023)
bits 51-0  : mantisa (52 bits, fracción implícita con 1 implícito)
```

El valor se calcula como: `(-1)^signo × 1.mantisa × 2^(exponente - 1023)`.

Valores especiales: exponente todo unos (2047) con mantisa 0 → `Infinity`; exponente todo unos con mantisa ≠ 0 → `NaN`; exponente y mantisa todo ceros → `0` (o `-0` si signo = 1).

El limite de 2^53 - 1 para enteros exactos se sigue de que la mantisa tiene 52 bits explícitos + 1 implícito = 53 bits de significando.

> [!tip] Buenas prácticas
> - Usar `Number.isNaN()` en lugar de `isNaN()` global.
> - Para comparaciones de decimales, usar epsilon o redondear previamente.
> - Usar el separador numérico `1_000_000` para legibilidad en literales grandes.
> - Para enteros fuera de `Number.MAX_SAFE_INTEGER`, usar [[07 bigint|bigint]].

> [!warning] Errores comunes
> - `NaN === NaN` → `false`; nunca comparar con `===`.
> - `typeof NaN === "number"` → `true`; no indica que el valor sea un número válido.
> - `-0 === 0` → `true`; para distinguirlos usar `Object.is(-0, 0)` → `false`.
> - Aritmética decimal: `0.1 + 0.2 !== 0.3`; nunca comparar doubles con `===` directamente.

## Notas relacionadas

- [[07 bigint|bigint]] — enteros de precisión arbitraria
- [[04 Conversión de Tipos/01 Conversión Explícita (Number, String, Boolean)|Conversión Explícita]]
- [[02 typeof|typeof]]
- [[01 Primitivos/index|Primitivos]]
