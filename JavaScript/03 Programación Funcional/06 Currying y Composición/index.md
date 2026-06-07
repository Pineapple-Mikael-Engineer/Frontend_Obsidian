---
title: Currying y Composición — Índice
aliases:
  - currying composición javascript
  - FP avanzado js
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
---

# Currying y Composición

Currying y composición son las técnicas avanzadas de la programación funcional que permiten construir funciones complejas a partir de funciones simples. Currying transforma una función de múltiples argumentos en una cadena de funciones unarias; la composición encadena funciones en secuencia, donde la salida de una es la entrada de la siguiente.

> [!definicion]
> **Currying** convierte `f(a, b, c)` en `f(a)(b)(c)`. Cada llamada devuelve una función que espera el siguiente argumento. **Composición** combina `f`, `g`, `h` en una nueva función tal que `(f ∘ g ∘ h)(x) = f(g(h(x)))`. Ambas técnicas operan exclusivamente con **funciones unarias puras** y producen nuevas funciones sin efectos secundarios.

```js
// Currying manual
const sumar = a => b => a + b;
const sumar10 = sumar(10);
sumar10(5);  // 15

// Composición (pipe — izquierda a derecha)
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);
const procesar = pipe(
  x => x * 2,
  x => x + 1,
  x => `resultado: ${x}`
);
procesar(5);  // "resultado: 11"
```

## Técnicas cubiertas

| Técnica | Qué hace | Nota |
|---|---|---|
| Currying | Transforma `f(a,b)` en `f(a)(b)`; especializa funciones | `01 Currying` |
| Partial Application | Fija algunos argumentos de una función | `02 Partial Application` |
| compose | Combina funciones de derecha a izquierda: `f(g(h(x)))` | `03 compose` |
| pipe | Combina funciones de izquierda a derecha: `h(g(f(x)))` | `04 pipe` |

## Hilo conductor

Currying y partial application crean funciones especializadas a partir de funciones genéricas. `compose` y `pipe` las ensamblan en pipelines. El orden es siempre: **especializar → componer → ejecutar**.

```
f genérica → currying/partial → f especializada → compose/pipe → pipeline
```

## Notas relacionadas

- [[01 Currying]]
- [[02 Partial Application]]
- [[03 compose]]
- [[04 pipe]]
- [[../05 Inmutabilidad Práctica/index|Inmutabilidad Práctica — Índice]]
- [[../index|Programación Funcional — Índice]]
