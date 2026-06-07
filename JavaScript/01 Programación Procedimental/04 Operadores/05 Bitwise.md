---
title: Operadores Bitwise — &, |, ^, ~, <<, >>, >>>
aliases:
  - operadores bitwise JS
  - bitwise operators
  - operadores bit a bit
  - bitmask JS
tags:
  - javascript
  - api/operador
  - operadores
objeto: global
tipo: operador
retorna: number (int32 con signo, excepto >>> que devuelve uint32)
muta: false
asincrono: false
draft: false
---

# Operadores Bitwise

> [!definicion] Operadores bitwise
> Operan sobre la representación binaria de enteros de 32 bits. JS convierte ambos operandos a `int32` (con `>>>` convierte el izquierdo a `uint32`) antes de operar, luego devuelve el resultado como número JS normal. Los valores de punto flotante se truncan.

```js
// Representación binaria:
// 5  = 00000000000000000000000000000101
// 3  = 00000000000000000000000000000011

5 & 3   // → 1   (AND bit a bit: 0101 & 0011 = 0001)
5 | 3   // → 7   (OR  bit a bit: 0101 | 0011 = 0111)
5 ^ 3   // → 6   (XOR bit a bit: 0101 ^ 0011 = 0110)
~5      // → -6  (NOT bit a bit: invierte todos los bits, luego complemento a 2)
5 << 1  // → 10  (shift left:  0101 << 1 = 1010)
5 >> 1  // → 2   (shift right con signo)
5 >>> 1 // → 2   (shift right sin signo)
```

## Tabla de operadores

| Operador | Nombre | Descripción |
|----------|--------|-------------|
| `&` | AND | 1 si ambos bits son 1 |
| `\|` | OR | 1 si algún bit es 1 |
| `^` | XOR | 1 si los bits son distintos |
| `~` | NOT | invierte todos los bits |
| `<<` | Shift left | desplaza bits a la izquierda, rellena con 0 |
| `>>` | Shift right con signo | desplaza a la derecha, replica el bit de signo |
| `>>>` | Shift right sin signo | desplaza a la derecha, rellena con 0 |

## `~` y el truco de indexOf

`~n` aplica complemento a dos: equivale a `-(n + 1)`.

```js
~0   // → -1
~1   // → -2
~-1  // → 0    ← el único valor que produce 0 (falsy)
```

Esto convierte el resultado de `indexOf` (que devuelve `-1` si no encuentra) en un valor falsy/truthy:

```js
const arr = ["a", "b", "c"];

// Sin truco:
if (arr.indexOf("b") !== -1) { /* encontrado */ }

// Con truco ~:
if (~arr.indexOf("b")) { /* truthy si encontrado */ }
if (!~arr.indexOf("b")) { /* falsy si NO encontrado */ }

// En JS moderno, preferir includes:
if (arr.includes("b")) { /* más legible */ }
```

El truco con `~` es un idiom histórico; `includes` es preferible hoy.

## Flags de permisos (bitmask)

Los bitwise son la forma estándar de manejar conjuntos de flags binarios en un solo entero.

```js
// Definir flags como potencias de 2
const READ    = 1 << 0;  // 0001
const WRITE   = 1 << 1;  // 0010
const EXECUTE = 1 << 2;  // 0100
const ADMIN   = 1 << 3;  // 1000

// Combinar permisos con |
let permisos = READ | WRITE;  // → 3 (0011)

// Comprobar permiso con &
(permisos & READ)    !== 0  // → true   (tiene READ)
(permisos & EXECUTE) !== 0  // → false  (no tiene EXECUTE)

// Añadir permiso con |
permisos |= EXECUTE;  // permisos = 0111

// Quitar permiso con & ~
permisos &= ~WRITE;   // permisos = 0101  (quita WRITE)

// Toggle permiso con ^
permisos ^= READ;     // invierte el bit de READ
```

## Truncamiento a entero con `| 0` y `~~`

Convertir a `int32` tiene el efecto secundario de truncar la parte decimal, lo que en contextos de rendimiento (canvas, WebAssembly) es más rápido que `Math.floor` o `Math.trunc`.

```js
3.9  | 0  // → 3    (trunca, no redondea)
-3.9 | 0  // → -3   (trunca hacia cero, igual que Math.trunc)
~~3.9     // → 3    (doble NOT: mismo efecto)
~~-3.9    // → -3

// Comparativa de legibilidad:
Math.trunc(3.9)  // → 3  (forma recomendada para producción)
3.9 | 0          // → 3  (más rápido en rutas críticas)

// CUIDADO: límite de int32
2147483648 | 0   // → -2147483648  (overflow de int32)
```

## Shift como multiplicación/división por potencias de 2

`<< n` multiplica por `2^n`; `>> n` divide (con truncamiento) por `2^n`. Solo para enteros en rango int32.

```js
1 << 10   // → 1024   (= 1 * 2^10)
256 >> 3  // → 32     (= 256 / 2^3)
7 >> 1    // → 3      (= Math.floor(7/2))
-7 >> 1   // → -4     (extiende el bit de signo)
-7 >>> 1  // → 2147483644  (sin signo: trata -7 como uint32 y desplaza)
```

## Casos de uso reales en JS moderno

```js
// Manipulación de píxeles en canvas (ImageData)
function grayscale(imageData) {
  const data = imageData.data;
  for (let i = 0; i < data.length; i += 4) {
    const avg = (data[i] + data[i+1] + data[i+2]) / 3 | 0;
    data[i] = data[i+1] = data[i+2] = avg;
  }
}

// Generación rápida de colores RGB desde entero
const color = 0xFF8800;
const r = (color >> 16) & 0xFF;  // → 255
const g = (color >> 8)  & 0xFF;  // → 136
const b = color          & 0xFF;  // → 0

// Hash simple (djb2)
function hash(str) {
  let h = 5381;
  for (let i = 0; i < str.length; i++) {
    h = ((h << 5) + h) ^ str.charCodeAt(i);
  }
  return h >>> 0;  // normalizar a uint32
}
```

## Cómo funciona por dentro

El motor aplica `ToInt32` (o `ToUint32` para `>>>`) a cada operando. `ToInt32` convierte a float64, luego trunca a entero, toma el módulo `2^32` y ajusta el rango a `[-2^31, 2^31-1]`. Por eso `Math.PI | 0 === 3` y `2**31 | 0 === -2**31`. El resultado de la operación se convierte de vuelta a float64 (tipo de los números JS).

En V8, cuando los valores son en realidad enteros pequeños (`Smi`, Small Integer), las operaciones bitwise se optimizan sin conversión.

> [!tip] Buenas prácticas
> - Para truncar a entero en código de producción, usar `Math.trunc` o `Math.floor` por legibilidad. Reservar `| 0` y `~~` para rutas críticas de rendimiento (canvas, DSP, WebAssembly).
> - Para flags de permisos, documentar cada constante con su valor binario en comentario.
> - Preferir `includes`, `has` u operadores de conjunto sobre `~indexOf` en código moderno.

> [!warning] Errores comunes
> **Overflow de int32:** todo operando se convierte a int32 antes de operar. Valores fuera de `[-2^31, 2^31-1]` producen resultados inesperados. Para enteros grandes usar `BigInt`.
>
> **`>>>` convierte a uint32:** `-1 >>> 0` devuelve `4294967295` (2^32-1), no `-1`. Útil para normalizar a positivo, pero sorprendente si no se espera.
>
> **`~` sobre `NaN` o `undefined`:** `ToInt32(NaN) === 0`, así que `~NaN === -1` (truthy). El truco `~indexOf` falla si el array contiene `undefined`.

## Notas relacionadas

- [[02 Aritméticos]] — `| 0` como alternativa a `Math.trunc`, shift vs multiplicación
- [[01 Asignación]] — operadores compuestos `&=`, `|=`, `^=`, `<<=`, `>>=`, `>>>=`
