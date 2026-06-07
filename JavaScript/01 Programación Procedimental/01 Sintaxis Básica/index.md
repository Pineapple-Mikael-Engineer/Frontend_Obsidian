---
title: Sintaxis Básica de JavaScript — Visión general
aliases:
  - Sintaxis básica JS
  - JS sintaxis
tags:
  - javascript
  - api/concepto
  - sintaxis
objeto: global
tipo: concepto
draft: false
---

# Sintaxis Básica de JavaScript

> [!definicion]
> La sintaxis básica de JavaScript define las reglas que rigen cómo el motor V8 (o SpiderMonkey, JavaScriptCore) tokeniza, parsea y ejecuta un programa. Cubre la distinción entre **expresiones** (producen valor) y **declaraciones** (instrucciones sin valor directo), el papel del **punto y coma** como terminador (real o inferido por ASI), el formato de los **comentarios**, y el **modo estricto** como capa de seguridad sobre el comportamiento permisivo histórico del lenguaje.

Un programa JS es una secuencia de **sentencias** (*statements*). Cada sentencia puede ser una declaración pura (`if`, `for`, `function`) o una **expression statement** — una expresión que se evalúa y cuyo valor se descarta (la forma más frecuente: llamar a una función).

```js
// Expression statement: la llamada produce un valor, pero se descarta
console.log("hola");  // → hola (imprime el efecto secundario)

// Declaración pura: el if no produce valor
if (true) { console.log("ok"); }

// Expresión en contexto de asignación: el valor SÍ se usa
const doble = (x) => x * 2;   // arrow function como expresión
const resultado = doble(5);    // → 10
```

## Conceptos cubiertos en esta sección

| Concepto | Pregunta que responde | Archivo |
|---|---|---|
| Declaraciones y Expresiones | ¿Qué produce valor y qué no? | `01 Declaraciones y Expresiones.md` |
| Punto y Coma | ¿Cuándo termina una sentencia? ¿Qué hace ASI? | `02 Punto y Coma.md` |
| Comentarios | ¿Cómo documentar código? ¿Qué es JSDoc? | `03 Comentarios.md` |
| Modo Estricto | ¿Qué prohíbe `use strict`? ¿Cuándo es automático? | `04 Modo Estricto (use strict).md` |

## Estructura general de un archivo JS

```js
"use strict";          // (1) modo estricto — opcional salvo en módulos (siempre activo)

// (2) Importaciones (ES6 modules)
import { suma } from "./utils.js";

// (3) Declaraciones y expresiones — el cuerpo del programa
const MAX = 100;       // declaración: const

function calcular(n) { // declaración: function
  return n * 2;        // return es declaración; `n * 2` es expresión
}

calcular(MAX);         // expression statement: llamada a función usada como sentencia
```

Un módulo ES6 (`<script type="module">`) es siempre estricto y tiene su propio scope; no contamina el scope global.

## Relación entre los conceptos

La distinción expresión/declaración determina dónde puede aparecer cada construcción: los sitios que esperan una **expresión** (después de `=`, dentro de `(...)`, argumento de función) no aceptan declaraciones como `if` o `for`. El punto y coma delimita sentencias — cuando falta, ASI puede insertarlo correctamente o en lugares inesperados. El modo estricto cambia cómo el motor trata asignaciones a variables no declaradas, `this`, palabras reservadas y otras situaciones silenciosamente problemáticas.

## Notas relacionadas

- [[01 Declaraciones y Expresiones]]
- [[02 Punto y Coma]]
- [[03 Comentarios]]
- [[04 Modo Estricto (use strict)]]
- [[02 Variables/index | Variables (var, let, const, hoisting)]]
- [[10 Módulos y Organización/01 Módulos ES6/index | Módulos ES6]]
