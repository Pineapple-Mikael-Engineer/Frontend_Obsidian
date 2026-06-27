---
title: ReferenceError — variable inexistente en el scope
aliases:
  - ReferenceError
  - reference error js
tags:
  - javascript
  - api/clase
  - errores
objeto: Error
tipo: clase
retorna: "-"
muta: false
asincrono: false
draft: false
order: 2
---

# ReferenceError

> [!definicion]
> `ReferenceError` se lanza cuando el motor intenta leer o escribir una variable que no existe en ningún scope de la cadena de alcance en el momento de la ejecución. La operación no puede completarse porque no hay binding al que referirse.

```js
try {
  console.log(variableInexistente);
} catch (err) {
  console.log(err instanceof ReferenceError); // → true
  console.log(err.message); // → "variableInexistente is not defined"
}
```

**`name`:** `"ReferenceError"`.

## Causas comunes

| Causa | Código | Mensaje de error |
|-------|--------|-----------------|
| Variable nunca declarada | `console.log(x)` | `x is not defined` |
| Acceso en la TDZ de `let`/`const` | `console.log(y); let y = 1` | `Cannot access 'y' before initialization` |
| `arguments` en arrow function | `const f = () => arguments` | `arguments is not defined` |
| Variable de módulo sin importar | usar `useState` sin import | `useState is not defined` |
| Typo en nombre de variable | `consle.log("x")` | `consle is not defined` |

## Temporal Dead Zone (TDZ)

Las declaraciones `let` y `const` tienen hoisting (el binding se crea al inicio del scope), pero permanecen en la **zona muerta temporal** hasta la línea de declaración. Acceder al binding dentro de la TDZ lanza `ReferenceError`, no `undefined`:

```js
{
  // TDZ de `valor` comienza aquí
  console.log(valor); // → ReferenceError: Cannot access 'valor' before initialization
  let valor = 42;
  // TDZ termina
  console.log(valor); // → 42
}
```

`var` no tiene TDZ: se inicializa como `undefined` desde el inicio del scope. Por eso el mismo patrón con `var` retorna `undefined` en lugar de lanzar.

## Modo estricto vs sloppy mode

En **sloppy mode**, asignar a una variable nunca declarada la crea como propiedad del objeto global:

```js
// sloppy mode (sin "use strict")
x = 5;           // crea window.x = 5, no lanza
console.log(x);  // → 5
```

En **strict mode**, la misma asignación lanza `ReferenceError`:

```js
"use strict";
x = 5; // → ReferenceError: x is not defined
```

Los módulos ES (`type="module"`) están en strict mode por defecto; la creación accidental de globales no ocurre.

## ReferenceError vs TypeError

La distinción es importante para diagnosticar rápido:

| Error | Causa | Ejemplo |
|-------|-------|---------|
| `ReferenceError` | El binding no existe en ningún scope | `foo` no declarada |
| `TypeError` | El binding existe pero el valor no soporta la operación | `null.prop`, llamar a un número |

Si el inspector muestra `is not defined`, el problema es de scope o de nombre. Si muestra `Cannot read properties of`, el binding existe y es `null`/`undefined`.

> [!tip]
> El mensaje `"X is not defined"` casi siempre se resuelve revisando el scope de declaración, el orden de imports, o un typo en el nombre. Las herramientas de linting (ESLint `no-undef`) detectan la mayoría de estos casos en tiempo de escritura.

> [!warning]
> En entornos con bundlers (webpack, Vite), las variables de un módulo no importado producen `ReferenceError` en runtime aunque el bundler no siempre los detecte estáticamente. TypeScript con `strict: true` captura muchos de estos errores antes de ejecutar.

## Notas relacionadas

- [[01 Tipos de Errores/index | Tipos de Errores]]
- [[01 Tipos de Errores/03 TypeError | TypeError]]
- [[02 try - catch - finally/01 try y catch | try y catch]]
