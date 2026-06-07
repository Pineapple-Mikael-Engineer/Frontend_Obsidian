---
title: Operadores — Mapa de la sección
aliases:
  - operadores JS
  - operators JS
tags:
  - javascript
  - api/concepto
  - operadores
objeto: global
tipo: concepto
draft: false
---

# Operadores

> [!definicion] Operadores
> Un operador es una construcción sintáctica que produce un valor a partir de uno o más operandos. En JS los operadores son expresiones: siempre devuelven algo. La elección del operador correcto determina semántica de tipos, cortocircuito, mutación y precedencia de evaluación.

Los operadores se agrupan en nueve familias. El motor los evalúa según **precedencia** (qué se evalúa primero) y **asociatividad** (de izquierda a derecha o al revés cuando tienen igual precedencia).

```js
// Precedencia: * antes que +
2 + 3 * 4   // → 14  (no 20)
(2 + 3) * 4 // → 20  (paréntesis anulan precedencia)

// Asociatividad derecha: **
2 ** 3 ** 2 // → 512  (= 2 ** 9, no 8 ** 2)
```

> [!tip] Paréntesis siempre
> La tabla de precedencia tiene 20 niveles. Ante cualquier duda, usar paréntesis: hacen la intención explícita y el código más legible sin coste de rendimiento.

## Familias de operadores

| Familia | Operadores principales | Precedencia relativa |
|---------|------------------------|----------------------|
| Agrupación | `()` | 21 — la más alta |
| Acceso / llamada | `.` `?.` `[]` `()` | 20–19 |
| Unarios | `!` `~` `+` `-` `typeof` `void` `delete` | 16 |
| Exponenciación | `**` | 15 — asociatividad derecha |
| Multiplicativos | `*` `/` `%` | 14 |
| Aditivos | `+` `-` | 13 |
| Shift bitwise | `<<` `>>` `>>>` | 12 |
| Relacionales | `<` `>` `<=` `>=` `in` `instanceof` | 11 |
| Igualdad | `==` `!=` `===` `!==` | 10 |
| Bitwise AND | `&` | 9 |
| Bitwise XOR | `^` | 8 |
| Bitwise OR | `\|` | 7 |
| Lógico AND | `&&` | 6 |
| Lógico OR | `\|\|` | 5 |
| Nullish | `??` | 4 |
| Ternario | `? :` | 3 — asociatividad derecha |
| Asignación | `=` `+=` `&&=` etc. | 2 — asociatividad derecha |
| Coma | `,` | 1 — la más baja |

## Notas hijas

- [[01 Asignación]] — `=`, compuestos, logical assignment, desestructuración como asignación
- [[02 Aritméticos]] — `+` `-` `*` `/` `%` `**`, `++`/`--`, NaN, Infinity
- [[03 Comparación]] — `===`/`!==`, `==`/`!=`, relacionales, `Object.is`
- [[04 Lógicos]] — `&&` `||` `!`, short-circuit, valor devuelto vs boolean
- [[05 Bitwise]] — `&` `|` `^` `~` `<<` `>>` `>>>`, bitmask, truncamiento
- [[06 Ternario]] — expresión condicional, cuándo usar vs `if/else`
- [[07 Nullish Coalescing (??)]] — `??` vs `||`, `??=`, combinación con `?.`
- [[08 Optional Chaining (?.)]] — `?.prop`, `?.[expr]`, `?.()`, encadenamiento seguro
- [[09 Operador in]] — pertenencia de propiedad, prototipo, `hasOwn`

## Cómo funciona por dentro

El parser de V8 construye un AST donde cada operador es un nodo con sus operandos como hijos. La precedencia está codificada en la gramática del lenguaje (spec ECMAScript §13), no es una tabla de consulta en tiempo de ejecución. La asociatividad determina cómo se agrupan los nodos cuando hay operadores del mismo nivel: la mayoría son left-associative (`a - b - c` → `(a - b) - c`); `**`, `? :` y `=` son right-associative.

> [!warning] Coerción implícita
> Muchos operadores JS aplican coerción de tipos silenciosa antes de operar (`+` con string, `==` con tipos distintos, operadores bitwise que convierten a int32). Las notas de [[03 Comparación]] y [[02 Aritméticos]] documentan los casos más sorprendentes.

## Notas relacionadas

- [[01 Variables y Tipos/index | Variables y Tipos]] — los valores sobre los que operan los operadores
- [[05 Control de Flujo/index | Control de Flujo]] — cómo los operadores lógicos y ternario interactúan con el flujo
