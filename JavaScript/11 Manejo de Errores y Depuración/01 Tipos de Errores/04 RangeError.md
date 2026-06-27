---
title: RangeError — valor fuera del rango permitido
aliases:
  - RangeError
  - range error js
tags:
  - javascript
  - api/clase
  - errores
objeto: Error
tipo: clase
retorna: "-"
muta: false
asincrono: false
draft: false
order: 4
---

# RangeError

> [!definicion]
> `RangeError` se lanza cuando un valor numérico o de conteo cae fuera del rango aceptable para una operación. El caso más frecuente en producción es la recursión sin caso base, que agota el call stack.

```js
function infinita() { return infinita(); }

try {
  infinita();
} catch (err) {
  console.log(err instanceof RangeError); // → true
  console.log(err.message); // → "Maximum call stack size exceeded"
}
```

**`name`:** `"RangeError"`.

## Causas principales

| Operación | Motivo del RangeError | Ejemplo |
|-----------|----------------------|---------|
| Recursión sin caso base | El call stack se llena completamente | `function f() { return f(); }` |
| `new Array(n)` con `n` negativo | Tamaño de array inválido | `new Array(-1)` |
| `new Array(n)` con `n >= 2**32` | Supera el límite de índice de array | `new Array(4294967296)` |
| `Number.prototype.toFixed(n)` con `n > 100` | Dígitos fuera del rango 0–100 | `(1.5).toFixed(200)` |
| `Number.prototype.toPrecision(n)` con `n > 100` | Precisión fuera del rango 1–100 | `(1).toPrecision(200)` |
| `String.prototype.repeat(n)` con `n < 0` o `Infinity` | Conteo de repeticiones inválido | `"a".repeat(-1)` |
| `new Date()` con timestamp `NaN` | Fecha inválida (a veces `TypeError`, depende del engine) | — |

## Call stack overflow

`"Maximum call stack size exceeded"` es el `RangeError` más frecuente en producción. Se produce cuando cada llamada recursiva apila un frame sin que ninguno se resuelva antes de añadir el siguiente:

```js
// Malo: sin caso base
function factorial(n) {
  return n * factorial(n - 1); // nunca termina para n <= 0 no entero
}

// Correcto: caso base explícito
function factorial(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}

// Para n muy grande: versión iterativa o trampolín
function factorialIter(n) {
  let acc = 1;
  for (let i = 2; i <= n; i++) acc *= i;
  return acc;
}
```

El límite exacto del call stack varía por engine y entorno: V8 (Chrome/Node) permite aproximadamente 10 000–15 000 frames; SpiderMonkey (Firefox) y JavaScriptCore (Safari) tienen límites similares.

## Creación segura de arrays de tamaño variable

```js
// Lanza RangeError si size es negativo:
const a = new Array(-1); // → RangeError: Invalid array length

// Guard:
function crearArray(size, fill = 0) {
  if (!Number.isInteger(size) || size < 0) {
    throw new RangeError(`Tamaño inválido: ${size}`);
  }
  return Array(size).fill(fill);
}
```

> [!tip]
> Ante un stack overflow inesperado, buscar en el stack trace si la misma función aparece repetida. Si sí, hay recursión mutua o directa sin condición de salida suficientemente fuerte. También puede originarse en `JSON.stringify` sobre objetos con referencias circulares (aunque ese caso lanza `TypeError` en V8 moderno).

> [!warning]
> `"Maximum call stack size exceeded"` no siempre indica recursión explícita. Puede originarse en listeners de eventos que se disparan mutuamente, en watchers reactivos circulares (Vue, MobX) o en serializadores que encuentran referencias circulares. El stack trace es la clave para encontrar el ciclo.

## Notas relacionadas

- [[01 Tipos de Errores/index | Tipos de Errores]]
- [[02 try - catch - finally/01 try y catch | try y catch]]
- [[03 throw/index | throw y errores personalizados]]
