---
title: IIFE como patrón de buenas prácticas
aliases:
  - IIFE moderno
  - Immediately Invoked Function Expression patrones
  - función autoejecutada moderno
tags:
  - javascript
  - api/concepto
  - patrones
draft: false
---

# IIFE

> [!definicion]
> Una **IIFE** (*Immediately Invoked Function Expression*) es una función que se define y ejecuta en el mismo momento, creando un scope léxico propio. Aunque los módulos ES6 resuelven el problema de contaminación de scope global en proyectos con bundler, la IIFE conserva utilidad en entornos sin bundler, en scripts inline, en polyfills y en situaciones donde se necesita async sin top-level await.

La sintaxis actual preferida usa arrow function:

```js
(() => {
  const CONFIG = { version: "1.0.0", debug: false };
  inicializar(CONFIG);
})();
```

La variante con `void` convierte la declaración en expresión sin el paréntesis envolvente:

```js
void function() {
  console.log("ejecutado");
}();
// `void` evalúa la expresión y devuelve undefined — efectivamente la llama
```

## Cuándo sigue siendo relevante en código moderno

| Escenario | Por qué IIFE |
|---|---|
| Script inline en HTML sin bundler | No hay scope de módulo; la IIFE confina las variables del script |
| Polyfill o snippet autocontenido | Debe funcionar en cualquier entorno sin dependencias de build |
| Inicialización con `async` sin top-level await | Permite usar `await` dentro cuando el entorno no soporta TLA |
| Configuración de librería de terceros | La IIFE garantiza que la configuración no expone estado al scope global |
| Resultado inmediato asignado a constante | Los bloques `{}` no retornan valor; la IIFE sí |

## IIFE async — sustituto de top-level await

En entornos que no soportan top-level await (Node.js < 14.8 sin `"type": "module"`, o scripts clásicos):

```js
(async () => {
  const datos = await fetch("/api/config").then(r => r.json());
  inicializarApp(datos);
})();
```

Con top-level await disponible (módulos ES6 modernos), la IIFE es innecesaria:

```js
// En un módulo ES6 (type="module" o bundler)
const datos = await fetch("/api/config").then(r => r.json());
inicializarApp(datos);
```

## IIFE vs bloque `{}` + `let`/`const`

Para el único propósito de limitar scope, un bloque de bloque es más simple:

```js
// Solo para limitar scope — bloque es suficiente
{
  let temporal = calcular();
  procesarResultado(temporal);
}
// temporal no existe aquí

// IIFE necesaria cuando se retorna un valor
const modulo = (() => {
  const privado = estado();
  return { obtener: () => privado };
})();
```

La IIFE sigue siendo la única opción cuando: (1) se necesita retornar un valor de la expresión, (2) se usa `async`/`await` sin soporte de TLA, o (3) se pasan dependencias globales explícitas como argumentos para minificación.

## Pasar dependencias como argumentos

Técnica de librerías pre-módulos para hacer dependencias explícitas y minificables:

```js
(function(global, doc) {
  function init() { return doc.querySelector(".app"); }
  global.MiLib = { init };
})(window, document);
// Los minificadores renombran 'window' → 'a', 'document' → 'b'
```

## Cómo funciona por dentro

El motor JS parsea `function() {}` como una **declaración de función**, que no puede seguirse de `()` para invocarla — el parser espera un identificador. El paréntesis envolvente `(function() {})` cambia el contexto de parseo: ahora es una **expresión de función**, un valor, sobre el que el `()` final sí puede aplicarse como operador de llamada. El operador `void` logra el mismo efecto semántico porque también transforma la declaración en expresión.

> [!tip]
> En proyectos con bundler (Vite, Webpack, Rollup), cada archivo es ya un módulo con scope propio: la IIFE no aporta nada. Su uso principal hoy es para scripts de configuración inline en HTML, snippets de monitorización (analytics, A/B testing) y polyfills que deben ser drop-in sin build step.

> [!warning]
> La arrow function IIFE no tiene `this` propio ni `arguments`. Si el código interno necesita `this` (por ejemplo, para encapsular métodos que se adjuntan a `window` o a un objeto receptor), usar `function` clásica. Además, la IIFE no es reemplazable por un bloque `{}` cuando se necesita retornar un valor: los bloques de bloque no son expresiones en JS.

## Notas relacionadas

- [[JavaScript/10 Módulos y Organización/02 Module Pattern (IIFE)/01 IIFE|IIFE (Module Pattern)]] — fundamentos del patrón y contexto histórico
- [[JavaScript/10 Módulos y Organización/02 Module Pattern (IIFE)/index|Module Pattern (IIFE)]]
- [[04 Memoización|Memoización]]
- [[05 Patrones de Diseño/index|Patrones de Diseño]]
