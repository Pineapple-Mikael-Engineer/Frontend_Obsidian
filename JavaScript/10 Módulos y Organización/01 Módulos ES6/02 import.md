---
title: import — Consumir exports de un módulo ESM
aliases:
  - named import
  - default import
  - namespace import
  - side-effect import
  - import estático
tags:
  - javascript
  - api/concepto
  - modulos
objeto: global
tipo: concepto
muta: false
asincrono: false
draft: false
order: 2
---

# import

> [!definicion]
> `import` declara las dependencias de un módulo y vincula identificadores locales a los exports de otro archivo. Es **estático**: debe aparecer en el nivel superior del módulo (no dentro de funciones, condicionales o bloques), y el motor de JS resuelve todas las declaraciones antes de ejecutar código. Para carga bajo demanda existe [[01 Módulos ES6/05 Import Dinámico | `import()` dinámico]].

```js
import { suma, PI } from "./math.js";
import parsear from "./parser.js";
import * as Utils from "./utils/index.js";

console.log(suma(PI, 1)); // → 4.14159
```

## Named import

Importa exports por nombre. El nombre local debe coincidir con el nombre exportado, salvo que se use alias.

```js
import { suma, PI } from "./math.js";
import { suma as add, PI as piVal } from "./math.js"; // alias local
```

## Default import

El nombre es libre: lo elige el módulo importador. No lleva llaves.

```js
import parsear from "./parser.js";      // nombre canónico
import miParser from "./parser.js";     // cualquier nombre es válido
```

## Namespace import

Importa todos los named exports como propiedades de un objeto. Útil para módulos con muchas exportaciones y cuando se quiere agrupar con un prefijo semántico.

```js
import * as Math from "./math.js";
console.log(Math.suma(1, 2)); // → 3
console.log(Math.PI);         // → 3.14159
// Math.default contiene el default export si existe
```

> [!warning]
> El namespace import no incluye el default export bajo ningún nombre predefinido — se accede como `Math.default`. Para usar el default con un nombre propio, combinar: `import MiClase, * as helpers from "./mod.js"`.

## Side-effect import

Ejecuta el módulo para sus efectos secundarios (registrar polyfills, configurar globals, cargar CSS en bundlers) sin importar ningún identificador.

```js
import "./polyfills.js";
import "./setup-globals.js";
```

## Formas de import

| Forma | Sintaxis | Cuándo usar |
|-------|----------|-------------|
| Named | `import { fn, val } from "..."` | Cuando se necesitan exports específicos |
| Named con alias | `import { fn as localFn } from "..."` | Evitar colisiones de nombres |
| Default | `import Algo from "..."` | El módulo tiene un valor principal |
| Default + named | `import Clase, { util } from "..."` | El módulo mezcla default y named |
| Namespace | `import * as NS from "..."` | Agrupar toda la API bajo un prefijo |
| Side-effect | `import "./archivo.js"` | Polyfills, configuración, efectos de carga |

## Resolución de rutas

En ESM nativo del navegador las rutas deben ser **URLs relativas** (empiezan con `./` o `../`) o **URLs absolutas**. El navegador no resuelve bare specifiers (nombres sin ruta).

```js
import { suma } from "./math.js";            // ✓ relativa
import { algo } from "https://cdn.x.com/mod.js"; // ✓ absoluta
import { algo } from "lodash";               // ✗ bare specifier — falla sin Import Map o bundler
```

Los bundlers (Vite, Webpack, Rollup) resuelven bare specifiers consultando `node_modules`. En el navegador sin bundler se necesita un [[01 Módulos ES6/04 Import Maps | Import Map]] para mapear nombres a URLs.

## Cómo funciona por dentro

`import` establece **live bindings** al scope del módulo exportador — no copia valores. El módulo se evalúa una sola vez aunque sea importado desde varios archivos; todos los importadores comparten el mismo binding. Esto garantiza que el estado interno de un módulo sea singleton.

```js
// a.js y b.js importan el mismo módulo "estado.js"
// Ambos ven la misma instancia de estado — no se crea una copia por importador
import { store } from "./estado.js";
```

> [!tip]
> El orden de los imports al inicio del archivo no afecta el orden de evaluación de los módulos — el motor construye el grafo completo antes de evaluar. El orden de evaluación respeta las dependencias (un módulo se evalúa antes que los que lo importan).

## Notas relacionadas

- [[01 Módulos ES6/index | Módulos ES6]]
- [[01 Módulos ES6/01 export (named y default) | export — named y default]]
- [[01 Módulos ES6/05 Import Dinámico | Import Dinámico]]
- [[01 Módulos ES6/04 Import Maps | Import Maps]]
