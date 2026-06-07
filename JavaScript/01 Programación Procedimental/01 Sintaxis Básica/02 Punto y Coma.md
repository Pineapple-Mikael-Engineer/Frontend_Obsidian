---
title: Punto y Coma — ASI y terminadores de sentencia
aliases:
  - ASI
  - Automatic Semicolon Insertion
  - semicolons JS
  - punto y coma JavaScript
tags:
  - javascript
  - api/concepto
  - sintaxis
objeto: global
tipo: concepto
draft: false
---

# Punto y Coma

> [!definicion]
> El punto y coma (`;`) es el terminador de sentencia en JavaScript. El motor puede insertarlo automáticamente mediante **ASI** (*Automatic Semicolon Insertion*), un conjunto de reglas del spec ECMAScript que opera durante el parsing. ASI no es un "corrector de errores" post-hoc: forma parte de la gramática formal del lenguaje y su comportamiento es determinista pero contraintuitivo en ciertos casos límite.

```js
// Con punto y coma explícito — sin ambigüedad
const a = 1;
const b = 2;
console.log(a + b);   // → 3

// Sin punto y coma — ASI los inserta en estos casos
const a = 1
const b = 2
console.log(a + b)    // → 3  (ASI correcto aquí)
```

## Las reglas de ASI (spec ECMAScript)

El parser inserta un `;` virtual en tres situaciones:

**Regla 1 — Token inesperado al final de línea:** cuando al parsear aparece un token que no puede continuar la sentencia actual y hay un salto de línea entre ambos, se inserta `;` antes del salto.

**Regla 2 — Antes de `}`:** cuando el siguiente token es `}` y el código previo es una sentencia incompleta sin `;`.

**Regla 3 — Final de archivo (EOF):** si el programa termina con código que el parser no puede completar como sentencia válida, se inserta `;`.

**Regla especial — Sentencias restringidas:** `return`, `throw`, `break`, `continue`, `yield`, y el operador `++`/`--` postfijo tienen una **gramática restringida**: si hay un salto de línea entre la palabra clave y su operando, ASI se inserta automáticamente *antes* del salto, separando la sentencia.

```js
// Regla especial con return:
function f() {
  return
    { valor: 1 }   // ← ASI inserta ; después de return
}
f()  // → undefined  (¡no el objeto!)

// Equivalente a:
function f() {
  return;          // ASI aquí
  { valor: 1 }     // código muerto
}
```

## Los casos peligrosos donde ASI traiciona

### 1. `return` con valor en la siguiente línea

```js
// ❌ Bug silencioso
function getConfig() {
  return
    {
      debug: true,
      timeout: 5000
    }
}
// getConfig() → undefined

// ✓ Correcto
function getConfig() {
  return {
    debug: true,
    timeout: 5000
  }
}
```

### 2. Línea que empieza con `(`

Una línea que empieza con `(` continúa la sentencia anterior como una **llamada a función**.

```js
// ❌ Bug: se interpreta como a(b)(c)
const a = 1
const b = 2
(function() { console.log("IIFE") })()

// El parser lo ve así:
// const a = 1; const b = 2(function() { ... })()
// → TypeError: 2 is not a function

// ✓ Solución: punto y coma defensivo al inicio de la línea problemática
;(function() { console.log("IIFE") })()
```

### 3. Línea que empieza con `[`

Una línea que empieza con `[` puede interpretarse como acceso por índice de la expresión anterior.

```js
// ❌ Bug clásico
const nombres = ["Ana", "Luis"]
["Ana", "Luis"].forEach(n => console.log(n))

// El parser lo ve así:
// const nombres = ["Ana", "Luis"]["Ana", "Luis"].forEach(...)
// → TypeError: Cannot read properties of undefined

// ✓ Solución
const nombres = ["Ana", "Luis"]
;["Ana", "Luis"].forEach(n => console.log(n))
// o simplemente usar punto y coma al final de la primera línea
```

### 4. Línea que empieza con `/`

Una línea que empieza con `/` puede interpretarse como el operador de división aplicado a la expresión anterior, no como un literal RegExp.

```js
const x = 5
/patrón/.test("texto")   // El parser puede leerlo como: const x = 5 / patrón / .test(...)
                          // → ReferenceError: patrón is not defined
```

### 5. Línea que empieza con un backtick

Una template literal al inicio de línea se interpreta como un tagged template de la expresión anterior.

```js
const fn = () => "hola"
`esto es un template`    // se lee como: fn()`esto es un template`
                          // → TypeError: fn(...) is not a function (si fn() devuelve string)
```

### 6. `++` y `--` postfijos

El operador postfijo no puede aparecer tras un salto de línea respecto a su operando: ASI inserta `;` antes de `++`/`--`, convirtiéndolo en prefijo de la *siguiente* expresión.

```js
let i = 5
i
++i        // ASI → `i; ++i;`  — el ++ es prefijo de i en la línea siguiente
console.log(i)  // → 6 (no 6 como postfijo, coincide aquí, pero el semántico es distinto)

// Caso donde importa:
let a = 5, b = 10
a
++b         // → a; ++b;  — a no se incrementa, b sí
```

## El debate: siempre `;` vs ASI-first

### Estilo "siempre punto y coma"

Regla simple: terminar cada sentencia con `;`. Adoptado por Google Style Guide, Airbnb Style Guide. Elimina toda ambigüedad de ASI.

```js
const x = 1;
const y = 2;
console.log(x + y);
```

### Estilo "ASI-first" (sin punto y coma)

Omitir `;` salvo en las líneas que empiezan con `(`, `[`, template literals, `/`, `+`, `-`, donde se añade un `;` defensivo al inicio. Adoptado por StandardJS, algunos proyectos Vue.

```js
const x = 1
const y = 2
console.log(x + y)

// El `;` defensivo antes de líneas peligrosas:
;(async function() {
  await fetch('/api')
})()
```

> [!info]
> Ambos estilos son igualmente válidos. Lo importante es la **consistencia** dentro del proyecto. Los linters (ESLint con regla `semi`) imponen el estilo elegido automáticamente.

## Tabla de tokens problemáticos al inicio de línea

| Token inicial | ¿ASI los separa? | Problema si no hay `;` previo |
|---|---|---|
| `(` | No | Se interpreta como llamada a la expresión anterior |
| `[` | No | Se interpreta como acceso por índice |
| backtick | No | Se interpreta como tagged template |
| `/` | No | Se interpreta como división |
| `+` | No | Se interpreta como suma |
| `-` | No | Se interpreta como resta |
| `{` | Depende del contexto | Generalmente sí (inicio de bloque) |
| Identificador | Sí | ASI inserta `;` — seguro |
| `const`/`let`/`var` | Sí | ASI inserta `;` — seguro |

> [!tip] Buenas prácticas
> - Elegir un estilo (con o sin `;`) y configurar ESLint para que lo imponga.
> - Si se usa estilo sin `;`, añadir un `;` defensivo al inicio de cualquier línea que empiece con `(`, `[`, un backtick (template literal), `/`, `+`, `-`.
> - Nunca poner el `{` de apertura de un objeto de retorno en la siguiente línea después de `return`: siempre en la misma.
> - Separar IIFE del código anterior con `;` o asignarlas a una variable.

> [!warning] Errores comunes
> - `return` + salto de línea + valor: retorna `undefined`, no el valor. Es el bug de ASI más frecuente en código real.
> - Dos expresiones consecutivas separadas solo por salto de línea donde la segunda empieza con `(` o `[`: se concatenan como una sola sentencia, causando un `TypeError` en runtime, no un `SyntaxError` — difícil de depurar.
> - Creer que ASI es infalible: en los 5 casos de tokens peligrosos arriba, ASI *no inserta* el punto y coma porque la gramática técnicamente admite la continuación.

## Notas relacionadas

- [[01 Declaraciones y Expresiones]]
- [[04 Modo Estricto (use strict)]]
- [[10 Módulos y Organización/02 Module Pattern (IIFE)/01 IIFE | IIFE]]
- [[07 Programación Asíncrona/06 Async _ Await/index | Async/Await]]
