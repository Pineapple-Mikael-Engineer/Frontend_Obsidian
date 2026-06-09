---
title: Module Pattern (IIFE) — Encapsulación pre-ESM en JavaScript
aliases:
  - Module Pattern
  - IIFE pattern
  - patrón módulo
tags:
  - javascript
  - api/concepto
  - modulos
draft: false
---

# Module Pattern (IIFE)

> [!definicion]
> El **Module Pattern** es la técnica pre-ESM de encapsular código JavaScript: una IIFE crea un scope léxico aislado y devuelve un objeto con la API pública. El código dentro de la IIFE es inaccesible desde el exterior; solo el objeto retornado está expuesto. Esta es la forma en que librerías como jQuery, Lodash, Backbone y Three.js organizan su código.

```js
const MiModulo = (function() {
  // scope privado — nada de esto es accesible desde fuera
  let _estado = 0;

  function _privada() { return _estado * 2; }

  // API pública — lo único visible
  return {
    obtener() { return _privada(); },
    incrementar() { _estado++; }
  };
})();

MiModulo.incrementar();
MiModulo.obtener(); // 2
MiModulo._estado;  // undefined
```

El patrón resuelve el mismo problema que los módulos ES6 pero de forma manual: evita contaminar el scope global, protege estado interno y controla qué interfaz se expone.

## Notas de esta sección

- [[02 Module Pattern (IIFE)/01 IIFE | IIFE — Immediately Invoked Function Expression]]
- [[02 Module Pattern (IIFE)/02 Encapsulación con Closures | Encapsulación con Closures]]
- [[02 Module Pattern (IIFE)/03 Patrón Revelador | Patrón Revelador (Revealing Module)]]

## Evolución y contexto

Antes de ESM (2015) y CommonJS (Node.js 2009), JS no tenía sistema de módulos. El código en múltiples archivos compartía el mismo scope global del navegador — cualquier `var` en el nivel superior era una propiedad de `window`. El Module Pattern fue la solución idiomática durante la era de los scripts de navegador (2008–2015).

| Característica | Module Pattern (IIFE) | ESM (import/export) |
|---|---|---|
| Privacidad | Closures | Scope de archivo |
| Singleton | Sí (por defecto) | Sí (módulos se evalúan una vez) |
| Tree-shaking | No (análisis estático imposible) | Sí |
| Múltiples instancias | Manual (función fábrica) | Manual (función fábrica) |
| Dependencias | Orden de `<script>` | Declaradas con `import` |
| Entorno | Navegador (global) | Navegador + Node.js |

> [!tip]
> El Module Pattern sigue siendo relevante para entender código legacy, librerías que soportan entornos sin bundler, y para comprender cómo los closures dan privacidad — el mismo mecanismo que usan los campos privados (`#campo`) de las clases modernas, solo que con sintaxis diferente.

> [!warning]
> El Module Pattern produce singletons: no se puede instanciar múltiples veces sin convertirlo en una función fábrica. Para código nuevo con bundler, ESM es siempre preferible: ofrece tree-shaking, dependencias explícitas y análisis estático.

## Notas relacionadas

- [[10 Módulos y Organización/index | Módulos y Organización]]
- [[01 Módulos ES6/index | Módulos ES6 (ESM)]]
- [[10 Módulos y Organización/03 Bundling (concepto) | Bundling (concepto)]]
