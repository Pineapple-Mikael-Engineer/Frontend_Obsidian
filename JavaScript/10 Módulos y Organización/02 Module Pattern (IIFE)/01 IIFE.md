---
title: IIFE — Immediately Invoked Function Expression
aliases:
  - Immediately Invoked Function Expression
  - función autoejecutada
  - función autoinvocada
tags:
  - javascript
  - api/concepto
  - modulos
draft: false
---

# IIFE

> [!definicion]
> Una **IIFE** (Immediately Invoked Function Expression) es una función que se define y ejecuta en el mismo momento. Crea un scope léxico propio que no contamina el scope exterior: las variables declaradas dentro no son accesibles desde fuera, y no colisionan con otras variables del mismo nombre en el ámbito global.

```js
(function() {
  var privado = "no accesible desde fuera";
  console.log("ejecutado de inmediato");
})();

// Con arrow function (ES6+)
(() => {
  const privado = "tampoco accesible";
})();

// Con valor de retorno
const resultado = (function() {
  return 42;
})();
// resultado === 42
```

## El paréntesis exterior

El paréntesis que envuelve la función no es opcional: convierte una **declaración de función** en una **expresión de función**. Las declaraciones de función no pueden invocarse inmediatamente — el parser las trata como `function nombre() {}`, no como un valor. Al envolverla en paréntesis, el parser la trata como expresión y el `()` final la invoca.

```js
// Error de sintaxis — declaración seguida de ()
function() { }()  // SyntaxError

// Válido — la función es una expresión
(function() { })()   // estilo Douglas Crockford
(function() { }())   // estilo alternativo (Airbnb legacy)
```

Ambas variantes son semánticamente equivalentes. La primera `(fn)()` es la más común.

## Pasar argumentos para evitar acceso a globales

Pasar referencias globales como argumentos explícitos es una práctica de librerías antiguas: hace que la dependencia sea visible y evita que la función cierre sobre `window` directamente.

```js
(function(global, doc, $) {
  // Aquí $ es el parámetro local — no depende del nombre global
  function init() { doc.querySelector(".app"); }
  global.MiLib = { init };
})(window, document, jQuery);
```

Esta técnica también permite que los minificadores renombren los parámetros (`window` → `a`, `document` → `b`) reduciendo el tamaño del bundle.

## Variantes

| Variante | Sintaxis | Cuándo se usa |
|---|---|---|
| Clásica (función nombrada) | `(function nombre() {})()` | Debugging — el nombre aparece en el stack trace |
| Clásica (anónima) | `(function() {})()` | Uso genérico pre-ES6 |
| Arrow function | `(() => {})()` | ES6+; no tiene `this` ni `arguments` propios |
| Con retorno asignado | `const m = (function() { return {}; })()` | Module Pattern — el objeto retornado es la API |
| Con argumentos | `(function(dep) {})(globalDep)` | Librerías que reciben dependencias globales |

## Antes de los módulos ES6

La IIFE fue el mecanismo estándar para evitar contaminar el scope global durante la era de los scripts de navegador. Cualquier variable `var` en el nivel superior de un script clásico se convierte en propiedad de `window`; envolverlo en una IIFE la confina.

```js
// Sin IIFE — versión peligrosa
var nombre = "MiLib";       // window.nombre = "MiLib"
var VERSION = "1.0.0";      // window.VERSION = "1.0.0"

// Con IIFE — encapsulado
(function() {
  var nombre = "MiLib";     // privado
  var VERSION = "1.0.0";    // privado
  window.MiLib = { VERSION }; // solo esto sale
})();
```

jQuery, Lodash, Three.js y Backbone siguen este patrón: toda la librería vive dentro de una IIFE y solo el objeto principal (`$`, `_`, `THREE`, `Backbone`) se adjunta a `window`.

## `var` vs `let`/`const` en scope moderno

Con `let` y `const` en un bloque `{}`, se obtiene scope de bloque sin necesidad de IIFE:

```js
// IIFE (pre-ES6)
(function() {
  var contador = 0;
})();

// Bloque con let (ES6+) — equivalente para scope
{
  let contador = 0;
}
// contador no está definido aquí
```

Sin embargo, la IIFE sigue siendo necesaria cuando se quiere retornar un valor o crear una API (el Module Pattern), porque los bloques `{}` no retornan valores en JS.

> [!tip]
> En código moderno con bundler, la IIFE es innecesaria: cada archivo es ya un módulo con scope propio. La IIFE es relevante para entender código legacy, para scripts sin bundler, y como mecanismo subyacente del [[02 Module Pattern (IIFE)/index | Module Pattern]].

> [!warning]
> La arrow function IIFE no tiene `this` propio ni `arguments`. Si el código interno depende de `this` (por ejemplo, para encapsular métodos de un objeto), usar `function` clásica. En el contexto del Module Pattern el `this` raramente importa porque la API se devuelve como objeto literal.

## Notas relacionadas

- [[02 Module Pattern (IIFE)/index | Module Pattern (IIFE)]]
- [[02 Module Pattern (IIFE)/02 Encapsulación con Closures | Encapsulación con Closures]]
- [[02 Module Pattern (IIFE)/03 Patrón Revelador | Patrón Revelador]]
