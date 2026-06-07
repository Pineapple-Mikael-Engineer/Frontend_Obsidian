---
title: Global Scope — El ámbito global en JavaScript
aliases:
  - global scope
  - ámbito global
  - variables globales
tags:
  - javascript
  - api/concepto
  - variables
tipo: concepto
draft: false
---

# Global Scope

> [!definicion]
> El **ámbito global** contiene las variables y funciones declaradas fuera de cualquier función o bloque. Son accesibles desde cualquier parte del programa. En el navegador, el objeto global es `window`; en Node.js es `global`; en cualquier entorno moderno, `globalThis` es el alias estándar que funciona en todos ellos.

```js
// En el nivel raíz de un script:
var version = "1.0";    // se adjunta a window.version
let entorno = "prod";   // NO se adjunta a window
const APP = "miApp";    // NO se adjunta a window

console.log(window.version); // "1.0"
console.log(window.entorno); // undefined
console.log(window.APP);     // undefined
```

## `var` en el global → propiedad de `window`

Una variable `var` declarada en el nivel superior de un script (no en un módulo ES) se convierte en propiedad enumerable del objeto global. Esto puede colisionar con propiedades existentes de `window`.

```js
var name = "Ana";        // window.name ya existe y es string — se sobreescribe
var status = "activo";   // window.status ya existe — colisión silenciosa

console.log(window.name);   // "Ana"
console.log(window.status); // "activo"
```

Propiedades de `window` que colisionan frecuentemente: `name`, `status`, `location`, `history`, `frames`, `parent`, `top`, `self`, `opener`.

## `let` y `const` en el global — sin adjunción a `window`

`let`/`const` en el ámbito global crean bindings en el *Declarative Environment Record* del Script Record, que es distinto del *Object Environment Record* respaldado por `window`. Son globales en el sentido de que son accesibles desde cualquier módulo del script, pero no aparecen como propiedades del objeto global.

```js
let idioma = "es";
const VERSION = "2.0";

console.log(idioma);          // "es"
console.log(VERSION);         // "2.0"
console.log(window.idioma);   // undefined
console.log(window.VERSION);  // undefined
console.log("idioma" in window); // false
```

## `globalThis` — el acceso estándar al objeto global

`globalThis` (ES2020) provee una referencia al objeto global independientemente del entorno de ejecución.

```js
// Navegador:
console.log(globalThis === window); // true

// Node.js:
console.log(globalThis === global); // true

// Patrón para código isomórfico:
globalThis.configuracion = { debug: false };
```

## Contaminación del ámbito global

Declarar variables en el global —especialmente con `var`— es un antipatrón en aplicaciones grandes porque:

- Cualquier parte del código puede modificarlas accidentalmente.
- Los nombres colisionan entre scripts diferentes cargados en la misma página.
- Dificulta el testeo (estado compartido entre tests).

### Cómo evitarlo

**Módulos ES (`type="module"`):** el código de un módulo tiene su propio ámbito; las declaraciones de nivel superior no son globales.

```html
<script type="module">
  const datos = cargar(); // NO es global, solo existe en este módulo
</script>
```

**IIFE (*Immediately Invoked Function Expression*):** patrón premodular para crear un ámbito privado.

```js
(function() {
  var privado = "solo aquí";
  // … todo el código de la librería
})();

console.log(typeof privado); // "undefined"
```

**Namespacing:** exponer solo un objeto al global en lugar de múltiples variables sueltas.

```js
// ❌ Varias variables globales:
var usuarioActual = null;
var configuracion = {};
var cache = [];

// ✅ Un solo namespace:
var MiApp = {
  usuarioActual: null,
  configuracion: {},
  cache: []
};
```

## Variables globales implícitas

Asignar a un identificador no declarado en modo no estricto crea una propiedad global de forma silenciosa — uno de los bugs más difíciles de rastrear.

```js
function f() {
  olvidéDeclarar = 42; // sin var/let/const → crea window.olvidéDeclarar
}
f();
console.log(window.olvidéDeclarar); // 42

// Con "use strict" — lanza ReferenceError en lugar de crear la global:
"use strict";
function g() {
  noDeclarada = 1; // ReferenceError: noDeclarada is not defined
}
```

> [!warning] Errores comunes
> **Colisión con propiedades de `window`:** variables globales con nombres comunes (`name`, `status`, `top`) sobreescriben propiedades del objeto `window` con semántica diferente. `window.name` siempre es un string; asignarle un número lo convierte a string silenciosamente.
> **Variables implícitas:** olvidar `let`/`const`/`var` en una asignación dentro de una función crea una variable global invisible. Usar `"use strict"` convierte este error en `ReferenceError`.

> [!tip] Buenas prácticas
> Usar módulos ES (`<script type="module">`) para eliminar el problema del ámbito global en aplicaciones modernas. En scripts clásicos, usar `"use strict"` y evitar `var` a nivel global. Exponer al global solo lo estrictamente necesario, agrupado bajo un namespace.

## Notas relacionadas

- [[index|Ámbito (scope)]]
- [[02 Function Scope]]
- [[03 Block Scope]]
- [[../01 var|var]]
- [[../02 let|let]]
