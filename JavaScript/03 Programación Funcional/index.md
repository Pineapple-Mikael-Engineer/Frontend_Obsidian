---
title: Programación Funcional en JavaScript
aliases:
  - FP javascript
  - functional programming js
tags:
  - javascript
  - api/concepto
draft: false
---

# Programación Funcional en JavaScript

JavaScript es un lenguaje **multi-paradigma**: permite programación procedimental, orientada a objetos y funcional. La FP en JS es **pragmática** — no requiere pureza absoluta ni tipos algebraicos, sino aplicar sus principios donde aportan valor: código más predecible, fácilmente testeable y composable.

> [!definicion]
> La programación funcional trata las funciones como ciudadanos de primera clase: se asignan a variables, se pasan como argumentos y se devuelven como valores. Se construye software componiendo funciones puras (sin efectos secundarios, sin estado compartido mutable) en lugar de ejecutar secuencias de instrucciones que modifican estado.

El hilo conductor de la sección: **funciones puras** (qué son y por qué importan) → **sin efectos secundarios** → **inmutabilidad** → **funciones de orden superior** → **composición** → **pipelines**.

## Estructura de la sección

| Subsección | Contenido | Notas |
|---|---|---|
| 01 Conceptos Fundamentales | Funciones puras, inmutabilidad, efectos secundarios, declarativo vs imperativo | 5 |
| 02 Funciones de Orden Superior | Funciones que reciben y retornan funciones | 2 |
| 03 Arrow Functions | Sintaxis, `this` léxico, cuándo usarlas | 4 |
| 04 Métodos de Array Funcionales | `map`, `filter`, `reduce`, `find`, `some`, `every`, `flat`, `flatMap` y más | 13 |
| 05 Inmutabilidad Práctica | Spread, `Object.assign`, `Array.from`, `concat/slice`, métodos inmutables ES2023 | 5 |
| 06 Currying y Composición | Currying, partial application, `compose`, `pipe` | 4 |

## Por qué FP en JavaScript

- Las funciones puras son trivialmente testeables: entrada → salida, sin mocks ni setup de estado.
- La inmutabilidad elimina una clase entera de bugs: los causados por mutación compartida de estado.
- `map`, `filter`, `reduce` son más declarativos que los bucles equivalentes: describen *qué* se obtiene, no *cómo* se itera.
- La composición de funciones permite construir transformaciones complejas desde piezas simples y reutilizables.

## Notas relacionadas

- [[01 Conceptos Fundamentales/index|Conceptos Fundamentales]]
- [[02 Funciones de Orden Superior/index|Funciones de Orden Superior]]
- [[03 Arrow Functions/index|Arrow Functions]]
- [[04 Métodos de Array Funcionales/index|Métodos de Array Funcionales]]
- [[05 Inmutabilidad Práctica/index|Inmutabilidad Práctica]]
- [[06 Currying y Composición/index|Currying y Composición]]
