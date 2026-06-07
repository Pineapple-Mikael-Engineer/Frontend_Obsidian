---
title: this Global — this fuera de cualquier función
aliases:
  - this global
  - globalThis
  - this en scope global
tags:
  - javascript
  - api/concepto
  - this
objeto: global
tipo: concepto
muta: false
asincrono: false
draft: false
---

# `this` Global

> [!definicion]
> En el ámbito global — fuera de cualquier función — `this` hace referencia al **objeto global del realm**. Su valor concreto depende del entorno de ejecución y, en módulos ES, de las reglas adicionales del módulo.

```js
// Navegador — script clásico (<script> sin type="module")
console.log(this === window);          // → true
console.log(this === globalThis);      // → true

// Node.js — módulo CommonJS (nivel de archivo)
console.log(this === module.exports);  // → true (al inicio, antes de reasignaciones)

// Node.js — módulo ES (type: "module" o extensión .mjs)
console.log(this);                     // → undefined
```

## Entorno por entorno

| Entorno | `this` global (nivel de archivo) | Notas |
|---|---|---|
| Navegador, script clásico | `window` | También `globalThis === window` |
| Navegador, módulo ES | `undefined` | Los módulos siempre corren en modo estricto |
| Node.js CommonJS | `module.exports` | Cambia si se reasigna `module.exports` |
| Node.js módulo ES | `undefined` | Igual que en el navegador |
| Web Worker | `self` | `globalThis === self` |
| Deno | `globalThis` | Sin `window`; `self` también disponible |

## `globalThis` — la forma portátil (ES2020)

`globalThis` es la referencia estándar al objeto global en cualquier entorno, sin condicionantes de módulo o plataforma.

```js
// Funciona en navegador, Node.js, Deno, workers — sin importar el tipo de módulo
console.log(globalThis);

// Patrón idiomático para polyfills o código isomórfico
const g = globalThis;
g.miVariable = 42;  // equivalente a window.miVariable en browser
```

## Modo estricto a nivel de script

En modo estricto activo mediante `"use strict"` a nivel de script (no de módulo), el `this` del scope del script sigue siendo el objeto global. La restricción `this === undefined` solo afecta a **funciones** llamadas sin receptor, no al scope superior.

```js
"use strict";

console.log(this === window); // → true  (scope de script, no de función)

function fn() {
  console.log(this);          // → undefined  (función en modo estricto sin receptor)
}
fn();
```

## Cómo funciona por dentro

> [!info]
> El spec ECMAScript define que al crear el **Global Execution Context**, el motor establece `ThisBinding` igual al **Global Object** del realm (el objeto que representa el entorno: `window`, `global`, `self`…). Este valor es fijo para la duración del contexto global. Los módulos ES crean su propio contexto de módulo con `ThisBinding = undefined`, lo que explica el comportamiento diferente sin necesidad de `"use strict"` explícito.

## Buenas prácticas

> [!tip]
> Usar siempre `globalThis` en lugar de `window`, `global` o `self` para código que deba ejecutarse en múltiples entornos. Permite que el mismo código funcione sin ramas condicionales de entorno.

## Errores comunes

> [!warning]
> Asumir que `this` en un módulo ES es `window` es el error más frecuente al portar código de scripts clásicos. En módulos, `this` a nivel de archivo es `undefined`; acceder a las APIs globales requiere `globalThis.fetch`, o simplemente `fetch` (visible por el scope global sin necesidad de `this`).

## Notas relacionadas

- [[index|this — Contexto de invocación]] — los 5 contextos y regla de prioridad
- [[02 this en Función]] — default binding cuando `this` no es el global esperado
- [[01 Programación Procedimental/01 Sintaxis Básica/04 Modo Estricto (use strict)|Modo Estricto (use strict)]] — cómo `"use strict"` modifica el binding en funciones
