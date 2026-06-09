---
title: export — Declarar exports de un módulo ESM
aliases:
  - export named
  - export default
  - named export
  - default export
  - re-export
  - barrel export
tags:
  - javascript
  - api/concepto
  - modulos
objeto: global
tipo: concepto
muta: false
asincrono: false
draft: false
---

# export (named y default)

> [!definicion]
> `export` declara qué valores de un módulo son accesibles para otros módulos. Hay dos formas: **named exports** (identificados por nombre, pueden ser varios por módulo) y **default export** (el valor principal del módulo, máximo uno). Los exports forman el contrato público del módulo; todo lo no exportado es privado al archivo.

```js
// utils.js — named exports
export const PI = 3.14159;
export function suma(a, b) { return a + b; }
export class Vector { constructor(x, y) { this.x = x; this.y = y; } }

// parser.js — default export
export default function parsear(texto) {
  return texto.trim().split("\n");
}
```

## Named exports

Se pueden declarar en línea o agrupados al final del archivo. El enfoque agrupado es preferible para módulos con muchos exports: agrupa la interfaz pública en un solo lugar.

```js
// Inline
export const HOST = "https://api.ejemplo.com";
export function obtenerUsuario(id) { /* ... */ }

// Agrupado al final (equivalente)
const HOST = "https://api.ejemplo.com";
function obtenerUsuario(id) { /* ... */ }
export { HOST, obtenerUsuario };

// Con alias — el importador usa el nombre exportado
export { obtenerUsuario as fetchUser };
```

## Default export

Exporta el valor "principal" del módulo. Solo puede haber uno. El importador puede asignarle cualquier nombre local.

```js
// Función nombrada como default
export default function parsear(texto) { return texto.split(","); }

// Clase como default
export default class HttpClient { /* ... */ }

// Objeto literal como default
const config = { timeout: 5000, retries: 3 };
export default config;

// Expresión anónima (solo válida con default)
export default (a, b) => a + b;
```

> [!warning]
> `export default` exporta el **valor actual** de la expresión en el momento de evaluación del módulo, no una referencia live binding. Para primitivos no supone diferencia, pero para objetos mutables el importador verá el mismo objeto (referencia compartida).

## Formas de export

| Forma | Sintaxis | Límite por módulo | Alias en export |
|-------|----------|-------------------|-----------------|
| Named inline | `export const x = 1` | Sin límite | No directamente |
| Named agrupado | `export { x, y }` | Sin límite | Sí: `{ x as xAlias }` |
| Default | `export default valor` | 1 | No aplica |
| Re-export named | `export { fn } from "./mod.js"` | Sin límite | Sí |
| Re-export todo | `export * from "./mod.js"` | Sin límite | No |
| Re-export default | `export { default } from "./mod.js"` | 1 | Sí |

## Re-exports (barrel exports)

Un archivo `index.js` que solo re-exporta es un **barrel**: consolida la API pública de una carpeta. El bundler aplica tree-shaking sobre los re-exports igualmente.

```js
// src/utils/index.js — barrel
export { suma, resta } from "./math.js";
export { formatear } from "./format.js";
export * from "./validators.js";

// Re-exportar el default de otro módulo con nombre nuevo
export { default as HttpClient } from "./http.js";
```

```js
// Uso desde fuera — importa de un único punto
import { suma, HttpClient } from "./src/utils/index.js";
```

## Cuándo usar default vs named

`export default` conviene cuando el módulo representa **una sola cosa** (un componente React, una clase, una función principal). Named exports convienen cuando el módulo es una **colección de utilidades** relacionadas: cada función tiene identidad propia, el importador elige qué necesita, y el bundler puede tree-shake lo no usado.

> [!tip]
> Mezclar ambos es válido: `export default` para el valor principal y named exports para utilidades auxiliares. Ejemplo: un módulo de fecha puede tener `export default class Fecha` y `export { formatearISO, esFestivo }`.

## Cómo funciona por dentro

ESM usa **live bindings**: el export no es una copia del valor sino un enlace al identificador en el scope del módulo. Si el módulo exportador modifica la variable (`count++`), el importador ve el valor actualizado. Esto distingue ESM de CommonJS, donde `module.exports` exporta el valor en el momento de la asignación.

```js
// contador.js
export let count = 0;
export function incrementar() { count++; }

// main.js
import { count, incrementar } from "./contador.js";
console.log(count); // → 0
incrementar();
console.log(count); // → 1  (live binding: ve el cambio)
```

## Notas relacionadas

- [[01 Módulos ES6/index | Módulos ES6]]
- [[01 Módulos ES6/02 import | import]]
- [[10 Módulos y Organización/03 Bundling (concepto) | Bundling (concepto)]]
