---
title: bigint — Enteros de precisión arbitraria
aliases:
  - bigint JS
  - BigInt
  - tipo bigint
tags:
  - javascript
  - api/concepto
  - tipos
objeto: BigInt
tipo: concepto
draft: false
---

# bigint

> [!definicion]
> `bigint` es un tipo primitivo de JavaScript para representar **enteros de precisión arbitraria**, sin límite de tamaño práctico. Se usa cuando los valores superan `Number.MAX_SAFE_INTEGER` (2^53 - 1) y se requiere exactitud entera. Los literales se escriben con el sufijo `n`.

```js
typeof 42n         // → "bigint"
typeof BigInt(42)  // → "bigint"

9007199254740991n + 1n   // → 9007199254740992n  (exacto)
9007199254740991 + 1     // → 9007199254740992   (exacto, en el límite)
9007199254740991 + 2     // → 9007199254740992   (incorrecto: number pierde precisión)
```

## Literales y construcción

```js
// Literal con sufijo n
0n
42n
-100n
9007199254740992n

// Separador numérico (ES2021)
1_000_000_000_000n

// Hexadecimal, octal, binario
0xFFn      // → 255n
0o77n      // → 63n
0b1010n    // → 10n

// Constructor BigInt()
BigInt(42)         // → 42n
BigInt("1000000")  // → 1000000n
BigInt(3.14)       // → RangeError (solo enteros)
```

## Operadores y operaciones

```js
10n + 3n    // → 13n
10n - 3n    // → 7n
10n * 3n    // → 30n
10n / 3n    // → 3n  (división entera, trunca hacia cero)
10n % 3n    // → 1n
2n ** 10n   // → 1024n
-10n        // → -10n
```

La división trunca (no redondea ni produce decimales):

```js
7n / 2n     // → 3n  (no 3.5n)
-7n / 2n    // → -3n  (trunca hacia cero)
```

## Mezcla con number: prohibida

No se puede operar directamente entre `bigint` y `number`. Cualquier operación mixta lanza `TypeError`.

```js
42n + 1        // → TypeError: Cannot mix BigInt and other types
42n + 1n       // → 43n  (correcto)
42n + BigInt(1) // → 43n  (correcto)
Number(42n) + 1 // → 43   (correcto, convertido a number)
```

Las comparaciones entre bigint y number con `==` sí funcionan (coerción implícita); con `===` no (tipos distintos):

```js
42n == 42    // → true   (coerción)
42n === 42   // → false  (tipos distintos)
42n > 40     // → true   (comparación relacional permite mezcla)
```

## Diferencias con number

| Aspecto | `number` | `bigint` |
|---|---|---|
| Precision entera | Hasta 2^53 - 1 | Arbitraria |
| Decimales | Si (IEEE 754) | No |
| `Infinity` | Si | No |
| `NaN` | Si | No |
| `Math.*` | Si | No (no compatibles) |
| Mixtura de tipos | — | TypeError si se mezcla con number |

```js
// bigint no tiene Infinity ni NaN
1n / 0n      // → RangeError: Division by zero

// Math no acepta bigint
Math.max(1n, 2n)   // → TypeError
```

## Conversión

```js
// bigint → number (puede perder precisión)
Number(9007199254740993n)   // → 9007199254740992  (pierde el último bit)

// number → bigint (solo si es entero)
BigInt(10)       // → 10n
BigInt(3.5)      // → RangeError

// bigint → string
String(42n)      // → "42"
(42n).toString() // → "42"
(255n).toString(16)  // → "ff"
```

## Casos de uso

**Criptografía**: algoritmos como RSA operan con enteros de cientos de bits, fuera del rango seguro de `number`.

```js
// Exponenciación modular (RSA) requiere bigint
function modPow(base, exp, mod) {
  let result = 1n;
  base = base % mod;
  while (exp > 0n) {
    if (exp % 2n === 1n) result = (result * base) % mod;
    exp = exp / 2n;
    base = (base * base) % mod;
  }
  return result;
}
modPow(2n, 10n, 1000n)   // → 24n
```

**IDs de 64 bits de bases de datos**: PostgreSQL, MySQL y otras devuelven IDs de 64 bits que JavaScript convierte incorrectamente a `number` si se usan directamente.

```js
// ID de 64 bits de una BD:
const id = BigInt("18446744073709551615");   // 2^64 - 1, representado exactamente
```

**Timestamps de alta precisión**: marcas de tiempo en nanosegundos (como las de Web Performance API o Node.js `process.hrtime.bigint()`).

```js
const inicio = process.hrtime.bigint();
// ...operacion...
const fin = process.hrtime.bigint();
console.log(`${fin - inicio} ns`);
```

> [!tip] Buenas prácticas
> - Usar `bigint` solo cuando se necesita: tiene overhead comparado con `number`.
> - Para IDs de BD de 64 bits, recibirlos como strings en JSON y convertir con `BigInt(str)`.
> - En comparaciones mixtas, convertir explícitamente antes de operar.

> [!warning] Errores comunes
> - `JSON.stringify({n: 42n})` → `TypeError`: `bigint` no es serializable en JSON por defecto. Convertir a string antes: `{n: String(42n)}`.
> - `42n / 2n === 21n`, no `21.5n`: la división es entera.
> - `Math.*` no acepta bigint; para operaciones matemáticas avanzadas se necesita implementación propia.
> - `typeof BigInt(42)` → `"bigint"` (minúscula); el constructor `BigInt` (mayúscula) existe pero no se invoca con `new`.

## Notas relacionadas

- [[01 number|number]] — el tipo para valores en rango seguro e IEEE 754
- [[04 Conversión de Tipos/01 Conversión Explícita (Number, String, Boolean)|Conversión Explícita]]
- [[01 Primitivos/index|Primitivos]]
