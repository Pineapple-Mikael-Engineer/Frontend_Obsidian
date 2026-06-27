---
title: Operador de Asignación — = y variantes compuestas
aliases:
  - asignación JS
  - assignment operator
  - logical assignment
tags:
  - javascript
  - api/operador
  - operadores
objeto: global
tipo: operador
retorna: el valor asignado
muta: true
asincrono: false
draft: false
order: 1
---

# Operador de Asignación

> [!definicion] Asignación
> `=` es una expresión que evalúa su operando derecho, almacena el resultado en el operando izquierdo (un lvalue: variable, propiedad, elemento de array) y **devuelve el valor asignado**. No es una declaración; produce un valor.

```js
let x = 5;          // x vale 5; la expresión (x = 5) devuelve 5
let y = (x = 10);   // x = 10, y = 10 — la asignación es una expresión
console.log(y);     // → 10
```

## Operadores compuestos

Combinan una operación aritmética, lógica o bitwise con la asignación. La forma `a OP= b` es equivalente a `a = a OP b` excepto que `a` se evalúa una sola vez (relevante si `a` tiene efectos secundarios).

| Operador | Equivalente | Descripción |
|----------|-------------|-------------|
| `+=` | `a = a + b` | suma / concatenación |
| `-=` | `a = a - b` | resta |
| `*=` | `a = a * b` | multiplicación |
| `/=` | `a = a / b` | división |
| `%=` | `a = a % b` | resto |
| `**=` | `a = a ** b` | exponenciación |
| `&=` | `a = a & b` | bitwise AND |
| `\|=` | `a = a \| b` | bitwise OR |
| `^=` | `a = a ^ b` | bitwise XOR |
| `<<=` | `a = a << b` | shift izquierdo |
| `>>=` | `a = a >> b` | shift derecho con signo |
| `>>>=` | `a = a >>> b` | shift derecho sin signo |

```js
let n = 10;
n += 5;   // → 15
n -= 3;   // → 12
n *= 2;   // → 24
n /= 4;   // → 6
n **= 3;  // → 216
n %= 100; // → 16
```

## Logical Assignment (ES2021)

`&&=`, `||=` y `??=` combinan la lógica de cortocircuito con la asignación. La asignación solo ocurre si el lado izquierdo satisface la condición; de lo contrario, no se escribe nada (ni siquiera el mismo valor).

| Operador | Asigna si... | Equivalente expandido |
|----------|-------------|----------------------|
| `&&=` | `a` es truthy | `a && (a = b)` |
| `\|\|=` | `a` es falsy | `a \|\| (a = b)` |
| `??=` | `a` es `null` o `undefined` | `a ?? (a = b)` |

```js
// &&=: asigna solo si ya existe un valor truthy
let user = { name: "Ana" };
user.name &&= user.name.trim(); // aplica trim porque name es truthy
// → user.name === "Ana"

let empty = "";
empty &&= "fallback"; // empty es falsy → no asigna
// → empty === ""

// ||=: valor por defecto si falsy
let config = { timeout: 0 };
config.timeout ||= 3000; // 0 es falsy → asigna 3000
// → config.timeout === 3000

// ??=: valor por defecto solo si null/undefined (0 y "" son válidos)
config.retries ??= 3; // retries era undefined → asigna 3
config.timeout ??= 5000; // timeout es 3000 (no nullish) → no asigna
// → config.timeout === 3000
```

## Desestructuración como asignación

La desestructuración también es una expresión de asignación. Sin `let`/`const`/`var`, opera sobre variables ya declaradas. El lado izquierdo es un patrón que extrae valores.

```js
// Array destructuring assignment
let a, b;
[a, b] = [1, 2];
console.log(a, b); // → 1 2

// Swap clásico sin variable temporal
[a, b] = [b, a];
console.log(a, b); // → 2 1

// Object destructuring assignment — requiere paréntesis
// (sin ellos, el parser interpreta { como bloque de código)
let x, y;
({ x, y } = { x: 10, y: 20 });
console.log(x, y); // → 10 20

// Desestructuración de parámetros con valor por defecto
function config({ host = "localhost", port = 3000 } = {}) {
  return `${host}:${port}`;
}
config({ port: 8080 }); // → "localhost:8080"
```

## Cómo funciona por dentro

El operador `=` en la spec de ECMAScript es una `AssignmentExpression`. El motor evalúa el lado derecho para obtener un valor, luego llama a `PutValue` con la referencia del lado izquierdo. PutValue distingue si es una variable del entorno actual, una propiedad de objeto (en cuyo caso llama al setter si existe) o un elemento de array. El valor que produce la expresión es el mismo que se asignó, lo que permite chaining: `a = b = c = 0` funciona porque `=` es right-associative y cada asignación devuelve el valor.

```js
// Chaining de asignaciones (right-associative)
let a, b, c;
a = b = c = 0;
// Se evalúa como: a = (b = (c = 0))
// c = 0 → devuelve 0; b = 0 → devuelve 0; a = 0
console.log(a, b, c); // → 0 0 0
```

> [!tip] Buenas prácticas
> - Preferir `??=` sobre `||=` cuando `0`, `""` o `false` son valores válidos que no deben sobreescribirse.
> - En desestructuración de objetos fuera de declaración, envolver en paréntesis para evitar ambigüedad con bloque.
> - Para inicializar propiedades opcionales de un objeto de configuración, `??=` es más seguro que `||=`.

> [!warning] Errores comunes
> **`=` en lugar de `===` dentro de `if`:** `if (x = 5)` siempre es truthy (y modifica `x`). Algunos linters (`no-cond-assign`) lo detectan. La forma segura es `if (x === 5)`.
>
> **`||=` con valores numéricos o vacíos:** `config.count ||= 10` sobreescribe `0` porque `0` es falsy. Usar `??=` si `0` es un valor legítimo.
>
> **Olvidar paréntesis en object destructuring assignment:** `{ x, y } = obj` produce SyntaxError porque `{` se interpreta como inicio de bloque. La forma correcta: `({ x, y } = obj)`.

## Notas relacionadas

- [[04 Lógicos]] — `&&`, `||` y `??` que gobiernan el cortocircuito de `&&=`, `||=`, `??=`
- [[07 Nullish Coalescing (??)]] — diferencia entre nullish y falsy para `??=`
- [[03 Comparación]] — por qué `===` es preferible a `=` en condiciones
