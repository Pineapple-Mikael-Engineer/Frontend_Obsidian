---
title: Bundling — Qué hace un bundler y por qué existe
aliases:
  - bundler
  - bundle
  - empaquetado JS
  - bundling JavaScript
tags:
  - javascript
  - api/concepto
  - modulos
draft: false
---

# Bundling (concepto)

> [!definicion]
> Un **bundler** es una herramienta que toma el grafo de módulos JS (y otros assets) de un proyecto y los combina en uno o pocos archivos optimizados para producción. El bundling resuelve dos problemas: los navegadores antiguos no soportan ESM nativo, y cargar cientos de módulos individuales genera cientos de peticiones HTTP con alta latencia.

```
src/
  main.js ──import──▶ utils.js ──import──▶ math.js
  main.js ──import──▶ api.js

              BUNDLER
                 ↓

dist/
  bundle.js  (todo en un archivo, minificado, tree-shaken)
```

## Qué hace un bundler

**Resolución de dependencias.** El bundler parsea los `import`/`require` de cada archivo, construye el árbol (grafo) de dependencias y determina qué módulos se necesitan y en qué orden.

**Concatenación.** Une los módulos en un único archivo (o un conjunto pequeño de chunks), envolviendo cada módulo en un scope aislado para evitar colisiones de nombres.

**Tree-shaking.** Elimina el código exportado que nunca se importa en ningún punto del grafo. Solo es posible con ESM estático: los `import`/`export` son analizables sin ejecutar el código. CommonJS (`require()`) es dinámico y no permite análisis estático completo.

```js
// math.js — exporta tres funciones
export function suma(a, b) { return a + b; }
export function resta(a, b) { return a - b; }
export function multiplicar(a, b) { return a * b; }  // nunca importada

// main.js
import { suma } from "./math.js";  // solo usa suma

// Bundle final — `resta` y `multiplicar` se eliminan por tree-shaking
```

**Minificación.** Elimina espacios, comentarios, acorta nombres de variables y aplica otras transformaciones que reducen el tamaño del archivo sin cambiar su comportamiento.

**Code splitting.** Divide el bundle en **chunks** que se cargan bajo demanda con `import()` dinámico. Evita cargar todo el código de la app en la primera visita.

```js
// Carga el módulo de gráficas solo cuando el usuario lo pide
button.addEventListener("click", async () => {
  const { renderChart } = await import("./charts.js");
  renderChart(data);
});
```

**Asset processing.** Con plugins o loaders, los bundlers transforman TypeScript → JS, JSX → JS, SCSS → CSS, optimizan imágenes, y gestionan hashes en nombres de archivo para cache busting.

## Bundlers principales

| Bundler | Año | Cuándo usar | Característica diferencial |
|---|---|---|---|
| Webpack | 2012 | Apps complejas con ecosistema de plugins maduro | El más completo; configuración verbose |
| Rollup | 2015 | Librerías (npm packages) | Tree-shaking excelente; output limpio |
| esbuild | 2020 | Herramienta base o builds muy rápidos | Escrito en Go; 10-100× más rápido que Webpack |
| Vite | 2020 | Apps web modernas (Vue, React, Svelte, Vanilla) | ESM nativo en dev (sin bundle); Rollup en prod |
| Parcel | 2017 | Proyectos pequeños o prototipado rápido | Zero-config; detecta el tipo de proyecto solo |
| Turbopack | 2022 | Next.js (sucesor de Webpack) | Incremental, escrito en Rust |

## Tree-shaking y CommonJS

El tree-shaking requiere que los módulos sean ESM. Con CommonJS, `require()` puede ejecutarse condicionalmente dentro de funciones o bloques `if`: el bundler no puede saber estáticamente qué se va a importar.

```js
// CommonJS — no tree-shakeable: require dinámico
const helper = require(condicion ? "./a" : "./b");
const { fn } = require("./utils");  // el bundler no sabe si fn se usa

// ESM — tree-shakeable: import estático
import { fn } from "./utils";  // el bundler sabe exactamente qué se usa
```

Por eso las librerías modernas publican en ESM además de CJS: `"main": "dist/index.cjs"`, `"module": "dist/index.esm.js"` en `package.json`. Los bundlers prefieren el campo `module` cuando está disponible.

## En desarrollo vs producción

Vite popularizó la distinción entre modo dev y modo producción:

- **Desarrollo:** sirve los módulos ES nativos directamente al navegador. El browser hace la resolución de módulos. Sin bundle → sin tiempo de compilación; el HMR (Hot Module Replacement) es casi instantáneo.
- **Producción:** usa Rollup para generar un bundle optimizado con tree-shaking, minificación y code splitting.

```
Dev:   browser ──ESM nativo──▶ módulos individuales servidos por Vite
Prod:  Rollup  ──bundle──────▶ dist/assets/index-abc123.js
```

## Cuándo no se necesita un bundler

- Proyectos pequeños con **Import Maps** + CDN: el browser resuelve los módulos directamente.
- Demos y prototipos rápidos con `<script type="module">`.
- Servidores Node.js con ESM: Node importa módulos sin necesidad de bundle.
- Aplicaciones Deno o Bun: el runtime resuelve URLs directamente.

> [!tip]
> Para proyectos nuevos, Vite es la opción por defecto: zero-config para la mayoría de frameworks, HMR instantáneo en desarrollo, y Rollup en producción. Rollup es preferible para publicar librerías en npm. Webpack sigue siendo relevante para aplicaciones grandes con configuración compleja ya establecida.

> [!warning]
> Tree-shaking solo funciona sobre imports en el **nivel superior** del módulo. Un `import` dentro de una función o condicional puede deshabilitar el tree-shaking para ese módulo. Algunos side effects (como código CSS importado desde JS) pueden hacer que el bundler no elimine un módulo aunque no se usen sus exports — se declaran en el campo `"sideEffects"` de `package.json`.

## Notas relacionadas

- [[10 Módulos y Organización/index | Módulos y Organización]]
- [[01 Módulos ES6/index | Módulos ES6 (ESM)]]
- [[02 Module Pattern (IIFE)/index | Module Pattern (IIFE)]]
