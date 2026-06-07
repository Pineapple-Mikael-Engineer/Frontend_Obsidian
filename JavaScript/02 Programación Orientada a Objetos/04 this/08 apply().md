---
title: Function.prototype.apply() — Invocar con this y argumentos en array
aliases:
  - apply
  - Function.prototype.apply
  - fn.apply
tags:
  - javascript
  - api/metodo
  - this
objeto: Function
tipo: metodo
retorna: any
muta: false
asincrono: false
draft: false
---

# `Function.prototype.apply()`

> [!definicion]
> `fn.apply(thisArg, argsArray)` invoca `fn` de forma inmediata con `this = thisArg` y argumentos proporcionados como **array** (o array-like). El mecanismo de binding es idéntico a `call` — la única diferencia es la forma de pasar los argumentos.

```js
function sumar(a, b, c) {
  return a + b + c;
}

const numeros = [10, 20, 30];

sumar.apply(null, numeros); // → 60
// equivalente a: sumar.call(null, 10, 20, 30)
// equivalente moderno: sumar(...numeros)
```

## Firma

```
fn.apply(thisArg[, argsArray])
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `thisArg` | any | Valor de `this` dentro de `fn` |
| `argsArray` | Array o array-like | Argumentos desempaquetados al invocar `fn` |
| **Retorna** | any | El valor devuelto por `fn` |

`argsArray` puede ser `null` o `undefined` — equivale a llamar sin argumentos.

## `call` vs `apply` vs spread

| Forma | Sintaxis | Cuándo usar |
|---|---|---|
| `call` | `fn.call(ctx, a, b, c)` | Argumentos individuales conocidos |
| `apply` | `fn.apply(ctx, [a, b, c])` | Array de argumentos ya construido |
| spread | `fn.call(ctx, ...arr)` o `fn(...arr)` | Moderno; preferido cuando ctx no importa |

`fn.apply(ctx, arr)` es semánticamente equivalente a `fn.call(ctx, ...arr)`.

## Casos de uso históricos — hoy reemplazados por spread

### `Math.max` / `Math.min` con array

```js
const notas = [7, 9, 5, 10, 8];

// Pre-ES6 (idiomático con apply)
Math.max.apply(null, notas); // → 10

// ES6+ (preferido)
Math.max(...notas);           // → 10
```

### Concatenar arrays sin mutar el original

```js
const a = [1, 2, 3];
const b = [4, 5, 6];

// Pre-ES6
Array.prototype.push.apply(a, b);
// → a = [1, 2, 3, 4, 5, 6]  — muta a (equivale a a.push(4, 5, 6))

// ES6+
a.push(...b); // mismo efecto, más legible
```

## Cuándo `apply` sigue siendo útil en código moderno

### El array proviene de una fuente dinámica y no se quiere usar spread

```js
function registrar(nivel, ...mensajes) {
  console[nivel](...mensajes);
}

// El array de argumentos viene de otra capa — apply es directo
Function.prototype.apply.call(console.log, console, mensajes);
```

### Decoradores y proxies que reenvían argumentos

```js
function con_log(fn) {
  return function (...args) {
    console.log("llamando con", args);
    return fn.apply(this, args); // conserva this y pasa todos los args
  };
}
```

El uso de `apply(this, args)` en wrappers y decoradores conserva simultáneamente el contexto y el array de argumentos capturado con rest — es el patrón estándar para funciones de orden superior transparentes.

## Cómo funciona por dentro

> [!info]
> Internamente, `apply` desempaqueta `argsArray` antes de construir el Execution Context — el motor produce la misma lista de argumentos que si se hubieran pasado individualmente. No hay penalización por el desempaquetado en motores modernos; V8, SpiderMonkey y JavaScriptCore optimizan ambas formas. `fn.apply(ctx, arr)` y `fn.call(ctx, ...arr)` producen exactamente el mismo bytecode en V8.

## Límite de argumentos (legado)

> [!warning]
> En motores antiguos (pre-2016), `apply` con arrays muy grandes (>65536 elementos en algunos entornos) causaba stack overflow porque los argumentos se colocaban en la pila de llamada. En motores modernos este límite ya no es un problema práctico, pero si se trabaja con arrays de millones de elementos es preferible iterar o usar `reduce` en lugar de `apply`/`spread`.

## Buenas prácticas

> [!tip]
> En código nuevo, preferir spread (`fn(...arr)`) sobre `apply` cuando `this` no es relevante — es más legible y no requiere pensar en el primer argumento. Reservar `apply(this, args)` para decoradores y wrappers donde el array `args` ya está construido y se necesita preservar el contexto.

## Errores comunes

> [!warning]
> Pasar un objeto no iterable como `argsArray` lanza `TypeError`. Verificar que el segundo argumento es un Array o al menos un array-like (tiene `length` y propiedades numéricas). `null` y `undefined` son los únicos valores no-array aceptados (significan "sin argumentos").

## Notas relacionadas

- [[index|this — Contexto de invocación]] — explicit binding y prioridades
- [[07 call()]] — versión con argumentos individuales; mecanismo idéntico
- [[09 bind()]] — variante que devuelve función en vez de invocarla
- [[03 Programación Funcional/02 Funciones de Orden Superior/index|Funciones de Orden Superior]] — decoradores y wrappers donde `apply` es idiomático
