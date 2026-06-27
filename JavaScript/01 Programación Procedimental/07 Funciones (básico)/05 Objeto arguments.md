---
title: Objeto arguments — Todos los argumentos en funciones no-arrow
aliases:
  - arguments object
  - objeto arguments
  - arguments
tags:
  - javascript
  - api/concepto
  - funciones
objeto: global
tipo: concepto
draft: false
order: 5
---

# Objeto `arguments`

> [!definicion]
> `arguments` es un objeto **array-like** disponible automáticamente dentro del cuerpo de toda función no-arrow. Contiene todos los argumentos pasados en la llamada, indexados desde `0`, con una propiedad `length`. No es un Array real: carece de métodos como `map`, `filter` o `reduce`. En modo estricto, no está sincronizado con los parámetros formales.

```js
function inspeccion() {
  console.log(arguments[0]);    // primer argumento
  console.log(arguments.length); // cantidad total
  console.log(Array.isArray(arguments)); // → false
}

inspeccion("a", "b", "c");
// → "a"
// → 3
// → false
```

## Propiedades del objeto

```js
function demo(x, y) {
  arguments[0]; // valor del primer argumento (= x en no-estricto)
  arguments[1]; // valor del segundo argumento (= y en no-estricto)
  arguments.length; // número de argumentos pasados (no de params declarados)
  arguments.callee; // referencia a la función (solo en no-estricto, deprecado)
}
```

`arguments.length` refleja los argumentos **pasados**, no los declarados. Si se declara `function f(a, b)` y se llama `f(1)`, `arguments.length` es `1` y `arguments[1]` es `undefined`.

## No existe en arrow functions

Las arrow functions no tienen su propio `arguments`. Si se referencia dentro de una arrow, se resuelve en el `arguments` del scope de función envolvente:

```js
function externa() {
  const interna = () => {
    console.log(arguments); // ← hereda de externa(), no de interna
  };
  interna("ignorado");
}

externa("a", "b"); // arguments = { 0: "a", 1: "b", length: 2 }
```

## Modo estricto vs no-estricto: sincronización

En **modo no-estricto**, `arguments[i]` y el parámetro formal `i` están sincronizados: modificar uno modifica el otro. Este comportamiento es sorprendente y fue eliminado en modo estricto:

```js
// MODO NO-ESTRICTO — sincronización activa
function noEstricto(x) {
  arguments[0] = 99;
  console.log(x); // → 99  (sincronizado)
}

// MODO ESTRICTO — sin sincronización
function estricto(x) {
  "use strict";
  arguments[0] = 99;
  console.log(x); // → 1  (no sincronizado)
}

noEstricto(1);
estricto(1);
```

## Conversión a Array real

Para aplicar métodos de Array, `arguments` debe convertirse primero:

```js
function sumarTodos() {
  // Forma moderna (ES6+)
  const nums = Array.from(arguments);
  return nums.reduce((a, b) => a + b, 0);
}

function sumarTodos2() {
  // Con spread
  return [...arguments].reduce((a, b) => a + b, 0);
}

function sumarTodos3() {
  // Forma clásica (ES5, compatible con entornos muy viejos)
  var nums = Array.prototype.slice.call(arguments);
  return nums.reduce(function(a, b) { return a + b; }, 0);
}

sumarTodos(1, 2, 3, 4); // → 10
```

## Cuándo se encuentra `arguments` hoy

`arguments` es legacy. Los [[04 Rest Parameters]] lo reemplazan con una API más clara y potente. Se encuentra en:

- Código ES5 y anterior (pre-2015).
- Polyfills y librerías antiguas.
- Situaciones donde se necesita inspeccionar argumentos en una función no-arrow sin rest params (extremadamente raro en código nuevo).

```js
// Pre-ES6 — patrón clásico variádico
function logLegacy() {
  var args = Array.prototype.slice.call(arguments);
  args.forEach(function(msg) { console.log(msg); });
}

// ES6+ — reemplazar por rest
function logModerno(...mensajes) {
  mensajes.forEach(msg => console.log(msg));
}
```

## Cómo funciona por dentro

El motor crea el objeto `arguments` en el **registro de activación** (execution context) de cada función no-arrow en el momento de la llamada. En modo no-estricto, el motor implementa los índices de `arguments` como accessors (getters/setters) vinculados a los parámetros formales del mismo scope. En modo estricto, `arguments` es un snapshot plano de los valores en el momento de la llamada: los índices son propiedades simples y no tienen vínculo con los parámetros.

`arguments.callee` (referencia a la función desde dentro) también desaparece en modo estricto porque dificulta las optimizaciones del motor (como inlining).

> [!tip] Buenas prácticas
> - En código nuevo, usar siempre `...rest` en lugar de `arguments`.
> - Si se mantiene código legacy con `arguments`, activar siempre `"use strict"` para desactivar la sincronización sorpresiva.
> - No usar `arguments.callee` — está deprecado y lanza TypeError en modo estricto.

> [!warning] Errores comunes
> **Usar `arguments` en una arrow function esperando los argumentos de esa arrow**:
> ```js
> const f = () => {
>   console.log(arguments); // ReferenceError si no hay función envolvente con arguments
> };
> f(1, 2, 3); // Error o hereda del scope externo — nunca contiene los args de f
> ```
> **Pasar `arguments` a otra función sin convertir** — `arguments` no es un Array, lo que rompe métodos que esperan un iterable real en algunos entornos:
> ```js
> function f() {
>   otroFn(arguments); // otroFn recibe un objeto, no un array
>   otroFn(...arguments); // correcto: spread convierte a argumentos separados
>   otroFn(Array.from(arguments)); // correcto: pasa un Array real
> }
> ```

## Notas relacionadas

- [[07 Funciones (básico)/index | Funciones (básico)]]
- [[04 Rest Parameters]]
- [[JavaScript/01 Programación Procedimental/01 Sintaxis Básica/04 Modo Estricto (use strict) | Modo Estricto]]
- [[JavaScript/03 Programación Funcional/03 Arrow Functions/03 Sin arguments ni new | Arrow Functions — Sin arguments ni new]]
