---
title: throw — mapa de la sección
aliases:
  - throw js
  - lanzar errores javascript
tags:
  - javascript
  - api/concepto
  - errores
objeto: throw
tipo: sentencia
retorna: "-"
muta: false
asincrono: false
draft: false
---

# throw

> [!definicion]
> `throw expresión` interrumpe la ejecución inmediatamente y propaga el valor lanzado hacia arriba por el call stack, buscando el bloque `catch` más cercano que pueda manejarlo. Si ninguno lo captura, el programa termina con un error no capturado. Por convención universal se lanza siempre un objeto `Error` o una subclase — nunca un string o número primitivo.

```js
throw new Error("algo salió mal");        // correcto — tiene .message y .stack
throw new TypeError("se esperaba string"); // correcto — tipo semántico
throw "error";                             // evitar — sin stack trace
throw 42;                                  // evitar — sin información útil
```

## Contenido de la sección

| Nota | Qué cubre |
|------|-----------|
| 01 Lanzar Errores | Sintaxis de `throw`, tipos lanzables, re-lanzar, `error.cause` |
| 02 Errores Personalizados (extends Error) | Subclases, jerarquía de dominio, `instanceof`, patrón TypeScript |

## Relación con el resto de la sección 11

El flujo completo: los errores se propagan por el call stack → [[02 try - catch - finally/index | try/catch/finally]] los captura → `throw` los relanza o crea nuevos → [[04 Depuración/index | las herramientas de depuración]] ayudan a entender qué pasó.

## Notas relacionadas

- [[03 throw/01 Lanzar Errores | Lanzar Errores]]
- [[03 throw/02 Errores Personalizados (extends Error) | Errores Personalizados (extends Error)]]
- [[02 try - catch - finally/index | try / catch / finally]]
- [[01 Tipos de Errores/index | Tipos de Errores nativos]]
