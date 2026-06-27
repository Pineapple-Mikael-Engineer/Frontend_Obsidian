---
title: Módulos ES6 — Sistema de módulos nativo de JavaScript
aliases:
  - ESM
  - ES Modules
  - módulos nativos JS
tags:
  - javascript
  - api/concepto
  - modulos
objeto: global
tipo: concepto
draft: false
order: 1
---

# Módulos ES6

> [!definicion]
> Los módulos ES6 (ESM — ECMAScript Modules) son el sistema de módulos nativo de JavaScript. Cada archivo es un módulo con su propio scope léxico: las declaraciones no son globales y no pueden colisionar entre archivos. Las dependencias entre módulos se declaran estáticamente con `import` y `export`, lo que permite al motor de JS resolver el grafo de dependencias antes de ejecutar código.

```js
// math.js — declara exports
export const PI = 3.14159;
export function suma(a, b) { return a + b; }

// main.js — declara imports
import { PI, suma } from "./math.js";
console.log(suma(1, 2)); // → 3
```

El grafo de módulos se construye en tres fases: **construcción** (parseo y resolución de rutas), **instanciación** (enlace de exports e imports en memoria), **evaluación** (ejecución del código). Los módulos se evalúan solo una vez aunque múltiples archivos los importen.

## Ventajas frente a scripts clásicos

- **Encapsulación de scope** — las variables del módulo no están en `window`; no hay contaminación global.
- **Tree-shaking** — los bundlers (Vite, Rollup) eliminan exports no usados porque las dependencias son estáticas y analizables.
- **Carga diferida** — `<script type="module">` es `defer` por defecto; no bloquea el renderizado.
- **Import dinámico** — `import()` permite code splitting: cargar un módulo solo cuando se necesita.
- **Ejecución en modo estricto** — los módulos corren siempre en `"use strict"` implícito.

## Notas de esta sección

- [[01 Módulos ES6/01 export (named y default) | export — named y default]]
- [[01 Módulos ES6/02 import | import]]
- [[01 Módulos ES6/03 Módulos en el Navegador (type=module) | Módulos en el Navegador (type=module)]]
- [[01 Módulos ES6/04 Import Maps | Import Maps]]
- [[01 Módulos ES6/05 Import Dinámico | Import Dinámico]]

## Comparación con sistemas anteriores

| Sistema | Entorno | Carga | Scope propio | Estático/Dinámico |
|---------|---------|-------|-------------|-------------------|
| Scripts clásicos | Navegador | Síncrona | No (global) | — |
| CommonJS (require) | Node.js | Síncrona | Sí | Dinámico |
| AMD (RequireJS) | Navegador | Asíncrona | Sí | Dinámico |
| ESM (import/export) | Navegador + Node | Asíncrona/defer | Sí | Estático + dinámico opcional |

> [!tip]
> El soporte nativo de ESM en navegadores modernos es completo desde 2018. En Node.js se usa con extensión `.mjs` o con `"type": "module"` en `package.json`. Los bundlers (Vite, Webpack, Rollup) transforman ESM para compatibilidad con entornos más antiguos.

> [!warning]
> Los módulos en el navegador requieren un servidor HTTP — no funcionan con `file://` por restricciones CORS. Para desarrollo local: `npx serve .`, `python -m http.server`, o cualquier servidor estático.

## Notas relacionadas

- [[10 Módulos y Organización/index | Módulos y Organización]]
- [[02 Module Pattern (IIFE)/index | Module Pattern (IIFE)]]
- [[10 Módulos y Organización/03 Bundling (concepto) | Bundling (concepto)]]
