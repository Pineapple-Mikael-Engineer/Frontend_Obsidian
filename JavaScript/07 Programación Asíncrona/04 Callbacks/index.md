---
title: Callbacks — patrón asíncrono original de JavaScript
aliases:
  - callbacks js
  - callback pattern
tags:
  - javascript
  - api/concepto
  - asincrono
draft: false
order: 4
---

# Callbacks

> [!definicion]
> Un **callback** es una función pasada como argumento a otra función que la invoca más tarde, cuando una operación termina. Es el mecanismo asíncrono original de JavaScript: todo el ecosistema de Node.js y las APIs del navegador (eventos, timers, XHR) se construyeron sobre él. Evolucionó hacia Promises y async/await para resolver sus limitaciones de composición y manejo de errores.

Los callbacks pueden ser **síncronos** (se ejecutan dentro del mismo stack, como el de `forEach`) o **asíncronos** (se ejecutan en una iteración futura del Event Loop, como el de `setTimeout` o `fs.readFile`). La distinción importa para razonar sobre el flujo de ejecución.

## Temas de esta sección

| Nota | Contenido |
|---|---|
| 01 Patrón Básico | Qué es un callback, síncronos vs asíncronos, inversión de control |
| 02 Callback Hell | Anidamiento profundo, "pyramid of doom", por qué surgieron las Promises |
| 03 Error-First Pattern | Convención de Node.js: error como primer argumento, promisify |

> [!tip] Contexto histórico
> Los callbacks no son malos en sí mismos — son el modelo más simple de comunicación asíncrona. Los problemas aparecen cuando se necesita **componer** múltiples operaciones asíncronas en secuencia o en paralelo. Entender sus limitaciones es entender por qué Promises y async/await existen.

> [!warning] Aún son relevantes
> Aunque async/await domina el código moderno, los callbacks siguen siendo la base de los event listeners del DOM, las APIs de Node.js nativas (fs, http, crypto), y cualquier API de terceros que no devuelva Promises. Saber leerlos y escribirlos correctamente sigue siendo necesario.

## Notas relacionadas

- [[01 Patrón Básico|Patrón Básico — qué es un callback]]
- [[02 Callback Hell|Callback Hell — pyramid of doom]]
- [[03 Error-First Pattern|Error-First Pattern — convención Node.js]]
