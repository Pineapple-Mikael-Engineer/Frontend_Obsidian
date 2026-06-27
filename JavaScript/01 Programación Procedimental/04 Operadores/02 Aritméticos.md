---
title: Operadores Aritméticos — +, -, *, /, %, **
aliases:
  - operadores aritméticos JS
  - arithmetic operators
  - módulo JS
  - resto JS
tags:
  - javascript
  - api/operador
  - operadores
objeto: global
tipo: operador
retorna: number (o string con +)
muta: false
asincrono: false
draft: false
order: 2
---

# Operadores Aritméticos

> [!definicion] Operadores aritméticos
> Producen un valor numérico a partir de sus operandos, aplicando las reglas de coerción de tipos de ECMAScript. La excepción es `+`, que también actúa como concatenación cuando algún operando es string.

```js
10 + 3   // → 13
10 - 3   // → 7
10 * 3   // → 30
10 / 3   // → 3.3333...
10 % 3   // → 1   (resto, no módulo matemático)
2  ** 8  // → 256 (exponenciación, ES2016)
```

## La ambigüedad de `+`

`+` es el único operador aritmético con doble rol. La regla de precedencia de tipos es: si **alguno** de los dos operandos es string, JS convierte el otro a string y concatena. Para el resto de operadores (`-`, `*`, `/`, `%`, `**`) siempre convierte ambos operandos a número.

```js
"3" + 4         // → "34"   (4 se convierte a "4", concatena)
3 + "4"         // → "34"   (ídem)
3 + 4 + "5"     // → "75"   (3+4=7 primero, luego "7"+"5")
"3" + 4 + 5     // → "345"  ("3"+4="34", luego "34"+5="345")
"3" - 1         // → 2      (- fuerza conversión numérica)
"3" * "2"       // → 6      (* fuerza conversión numérica)
true + true     // → 2      (true → 1)
null + 1        // → 1      (null → 0)
undefined + 1   // → NaN    (undefined → NaN)
```

> [!tip] Convertir a número con `+` unario
> El `+` unario (prefijo) convierte su operando a número: `+"42"` → `42`, `+true` → `1`, `+null` → `0`, `+undefined` → `NaN`. Es la conversión más concisa pero menos legible; `Number(x)` es preferible en código de producción.

## `%` — Resto, no módulo matemático

En JS, `%` calcula el **resto** de la división entera, con el signo del **dividendo** (operando izquierdo). Para operandos positivos coincide con el módulo matemático; para negativos, no.

```js
7  % 3   // → 1    (positivo)
-7 % 3   // → -1   (sigue el signo del dividendo: -7)
7  % -3  // → 1    (sigue el signo del dividendo: 7)
-7 % -3  // → -1
```

Para obtener el módulo matemático (siempre positivo), la receta es:

```js
function mod(n, m) {
  return ((n % m) + m) % m;
}
mod(-7, 3)  // → 2   (módulo matemático correcto)
mod(7, 3)   // → 1
```

## `**` — Exponenciación y asociatividad derecha

`**` tiene asociatividad derecha: `a ** b ** c` se evalúa como `a ** (b ** c)`. No como `(a ** b) ** c`.

```js
2 ** 3 ** 2   // → 512  (= 2 ** 9, porque 3 ** 2 = 9 primero)
(2 ** 3) ** 2 // → 64   (= 8 ** 2)

// No se puede anteponer un unario sin paréntesis:
// -2 ** 2  → SyntaxError (ambiguo)
(-2) ** 2    // → 4
-(2 ** 2)    // → -4
```

## `++` y `--` — Prefijo vs sufijo

`++` y `--` existen en dos formas con comportamiento diferente al usarse como expresión:

| Forma | Valor devuelto | Cuándo se aplica |
|-------|---------------|-----------------|
| `++i` (prefijo) | valor **después** del incremento | antes de devolver |
| `i++` (sufijo) | valor **antes** del incremento | después de devolver |

```js
let i = 5;
console.log(i++); // → 5   (devuelve 5, luego i = 6)
console.log(i);   // → 6
console.log(++i); // → 7   (incrementa a 7, devuelve 7)
console.log(i);   // → 7

// Efecto en bucles: ambas formas son equivalentes cuando no se usa el valor
for (let j = 0; j < 3; j++) { /* j++ o ++j da el mismo bucle */ }
```

En la mayoría de los bucles `for`, la diferencia no importa porque el valor devuelto se descarta. La distinción importa cuando `++` aparece dentro de una expresión más grande.

## NaN e Infinity

Toda operación aritmética que no produce un número finito válido devuelve `NaN` o `Infinity`. Estos valores se propagan.

```js
// Division por cero
1 / 0    // → Infinity
-1 / 0   // → -Infinity
0 / 0    // → NaN

// NaN se propaga
NaN + 1       // → NaN
NaN * 0       // → NaN
NaN === NaN   // → false  (NaN no es igual a sí mismo)
isNaN(NaN)    // → true   (pero convierte primero; preferir Number.isNaN)
Number.isNaN(NaN)    // → true
Number.isNaN("NaN")  // → false  (no convierte)

// Infinity aritmética
Infinity + 1    // → Infinity
Infinity - Infinity // → NaN
1 / Infinity    // → 0
```

## Recetas comunes

```js
// Truncar a entero (más rápido que Math.floor para positivos)
n | 0         // bitwise OR con 0: convierte a int32 y trunca
Math.trunc(n) // forma legible y sin límite de 32 bits

// Módulo matemático positivo
const mod = (n, m) => ((n % m) + m) % m;

// Promedio
const avg = arr => arr.reduce((s, x) => s + x, 0) / arr.length;

// Potencias de 2
2 ** 10  // → 1024
1 << 10  // → 1024 (equivalente bitwise, pero limitado a 32 bits)

// Comprobar si n es par/impar
n % 2 === 0 // par
n % 2 !== 0 // impar (ojo: -3 % 2 === -1, no 1; la condición sigue siendo !==0)
```

## Cómo funciona por dentro

El motor aplica el algoritmo `ToNumber` de la spec antes de operar. Para `+` binario, primero llama a `ToPrimitive` en ambos operandos (que puede invocar `valueOf` o `toString` de un objeto), y si alguno es string, el resultado es concatenación. Para los demás operadores aritméticos, `ToPrimitive` se llama con hint `"number"`. Los cálculos siguen IEEE 754 double-precision (64 bits), lo que explica imprecisiones como `0.1 + 0.2 !== 0.3`.

```js
// IEEE 754 double precision
0.1 + 0.2          // → 0.30000000000000004
(0.1 + 0.2).toFixed(1) // → "0.3"  (redondea para display)
Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON // → true  (comparación segura)
```

> [!warning] Errores comunes
> **Concatenación inesperada con `+`:** `"Total: " + 1 + 2` da `"Total: 12"`, no `"Total: 3"`. Agrupar: `"Total: " + (1 + 2)`.
>
> **`%` negativo:** `(-7) % 3` es `-1`, no `2`. Usar la receta `((n % m) + m) % m` si se necesita módulo matemático.
>
> **`NaN` silencioso:** una variable mal inicializada puede propagar `NaN` por toda una cadena de cálculos sin error. Validar entradas con `Number.isNaN` o `Number.isFinite`.
>
> **Pérdida de precisión con enteros grandes:** JS usa float64; enteros exactos hasta `Number.MAX_SAFE_INTEGER` (2^53 - 1). Para enteros mayores, usar `BigInt`.

## Notas relacionadas

- [[03 Comparación]] — `NaN === NaN` es `false`; `Object.is` para igualdad exacta
- [[05 Bitwise]] — truncamiento con `| 0`, potencias de 2 con shift
- [[01 Asignación]] — operadores compuestos `+=`, `-=`, `*=`, etc.
