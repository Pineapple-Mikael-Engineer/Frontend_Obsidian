---
title: Manejo de Errores y Depuración — mapa de la sección
aliases:
  - error handling javascript
  - manejo de errores js
  - debugging js
tags:
  - javascript
  - api/concepto
  - errores
draft: false
---

# Manejo de Errores y Depuración

> [!definicion]
> El manejo de errores en JavaScript cubre el ciclo completo: los errores se propagan por el call stack como objetos `Error`, `try/catch/finally` los captura y controla el flujo, `throw` los lanza o relanza con semántica de dominio, y las herramientas de depuración (DevTools, `console`, `debugger`) permiten inspeccionar qué ocurrió y dónde. Estas cuatro habilidades son inseparables en código de producción.

## Flujo de propagación

```
código lanza error
       ↓
  sube por el call stack
       ↓
¿hay un catch en el camino?
   sí → lo captura, ejecuta el handler, continúa
   no → Uncaught Error — el entorno (browser/Node) lo reporta
```

Un error no capturado en el browser se registra en consola y puede escucharse con `window.addEventListener("error", handler)` o `window.addEventListener("unhandledrejection", handler)` para promesas.

## Contenido de la sección

| Subdirectorio | Qué cubre |
|---------------|-----------|
| 01 Tipos de Errores | Tipos nativos (`TypeError`, `RangeError`, etc.), propiedades `name`, `message`, `stack` |
| 02 try - catch - finally | Sintaxis, ámbito del error, `finally` garantizado, manejo de promesas |
| 03 throw | Sentencia `throw`, re-lanzar errores, `error.cause`, errores personalizados con `extends Error` |
| 04 Depuración | `console` completo, breakpoints en DevTools, step, Watch Expressions |

## Subdirectorios

- [[01 Tipos de Errores/index | 01 Tipos de Errores]]
- [[02 try - catch - finally/index | 02 try / catch / finally]]
- [[03 throw/index | 03 throw]]
- [[04 Depuración/index | 04 Depuración]]

## Por qué importa

El código que no maneja errores explícitamente opera bajo la suposición de que ninguna operación puede fallar — suposición que se rompe con red inestable, inputs inesperados, APIs que cambian y límites del motor. Un sistema robusto distingue qué errores puede recuperar (reintentar, informar al usuario, degradar gracefully) de los que debe dejar propagarse (bugs internos que no deben silenciarse).

La depuración eficiente reduce el tiempo entre "el código no hace lo que debería" y "sé exactamente por qué y dónde". `console` y breakpoints cubren el 90% de los casos; el 10% restante requiere profiling, memory snapshots y network inspection — también en DevTools.
