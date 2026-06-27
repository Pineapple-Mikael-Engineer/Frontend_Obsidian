---
title: Módulos y Organización — JavaScript
aliases:
  - módulos JS
  - organización de código JS
  - JS modules
tags:
  - javascript
  - api/concepto
  - modulos
draft: false
order: 10
---

# Módulos y Organización

> [!definicion]
> Los módulos son unidades de código con scope propio que exponen una API pública y ocultan su implementación interna. JavaScript pasó de no tener sistema de módulos nativo a disponer de ESM (ECMAScript Modules), el estándar actual soportado por navegadores y Node.js. Esta sección cubre la evolución de esa historia y las herramientas de empaquetado que rodean al sistema de módulos.

## Evolución del sistema de módulos en JS

El problema de modularización existe desde los primeros programas JS de cierta complejidad: cómo dividir código en unidades reutilizables sin contaminar el scope global ni crear colisiones de nombres.

```
Scripts globales (< 2009)
  → todo en window, colisiones inevitables

Module Pattern / IIFE (≈ 2008–2015)
  → closures como scope privado, API retornada como objeto

CommonJS / require() (Node.js, 2009)
  → módulos síncronos para el servidor; no funciona en navegadores

AMD / RequireJS (≈ 2010–2014)
  → módulos asíncronos para el navegador; verboso

UMD (Universal Module Definition, ≈ 2011)
  → wrapper que detecta el entorno y usa CJS o AMD

ESM / import-export (ES2015+, soporte nativo ≈ 2018)
  → estándar estático, tree-shakeable, soportado en navegador y Node.js
```

## Elementos de esta sección

| Elemento | Tipo | Qué cubre |
|---|---|---|
| Módulos ES6 | Subsección | export, import, módulos en navegador, Import Maps, import dinámico |
| Module Pattern (IIFE) | Subsección | IIFE, closures para privacidad, Patrón Revelador |
| Bundling (concepto) | Nota standalone | Qué hace un bundler, tree-shaking, Vite / Rollup / Webpack / esbuild |

## Contenido

- [[01 Módulos ES6/index | Módulos ES6 (ESM)]] — el sistema de módulos estándar de JS: `import`, `export`, módulos en el navegador, Import Maps, import dinámico.
- [[02 Module Pattern (IIFE)/index | Module Pattern (IIFE)]] — encapsulación pre-ESM con IIFE y closures: privacidad por closure, Patrón Revelador.
- [[10 Módulos y Organización/03 Bundling (concepto) | Bundling (concepto)]] — qué hace un bundler, tree-shaking, code splitting, comparativa Vite / Rollup / Webpack.

## Criterios de elección en código nuevo

**ESM siempre** para código nuevo con bundler o entorno moderno (Node.js 14+, navegadores modernos). El Module Pattern queda para mantener código legacy o para scripts sin bundler donde no se puede usar `<script type="module">`.

Los bundlers (principalmente Vite para apps, Rollup para librerías) convierten el grafo de módulos ESM en un bundle optimizado para producción con tree-shaking, minificación y code splitting.

> [!tip]
> CommonJS (`require`) y AMD no se cubren en esta sección porque son sistemas que el código de aplicación moderno no escribe directamente — aparecen en configuraciones de herramientas (Jest, webpack.config.js) y en código de librerías que publican múltiples formatos. Para entenderlos en ese contexto es suficiente saber que son dinámicos (no tree-shakeables) y que los bundlers los soportan como entrada.

> [!warning]
> Los módulos ES6 en el navegador requieren servidor HTTP — no funcionan con `file://`. Para desarrollo: `npx serve .` o cualquier servidor estático. Los bundlers (Vite) gestionan esto automáticamente.

## Notas relacionadas

- [[JavaScript/index | JavaScript]]
