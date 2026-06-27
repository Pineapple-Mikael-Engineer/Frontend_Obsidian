---
title: this — Contexto de invocación
aliases:
  - this
  - contexto de this
  - this binding
tags:
  - javascript
  - api/concepto
  - this
objeto: global
tipo: concepto
draft: false
order: 4
---

# `this` — Contexto de invocación

> [!definicion]
> `this` es una referencia al **objeto de contexto** desde el que se invoca una función. Su valor no se determina en la definición de la función sino en el **momento de la llamada** — con la única excepción de las arrow functions, que capturan `this` del ámbito léxico circundante.

```js
function quienSoyYo() {
  console.log(this);
}

const obj = { nombre: "obj", quienSoyYo };

quienSoyYo();          // → window (non-strict) | undefined (strict)
obj.quienSoyYo();      // → { nombre: "obj", quienSoyYo: f }
quienSoyYo.call("X");  // → String{"X"} (non-strict) | "X" (strict)
```

## Los 5 contextos de binding

| Contexto | Cómo se activa | Valor de `this` (modo estricto) | Valor de `this` (no estricto) |
|---|---|---|---|
| **Global** | Ámbito superior, fuera de función | Objeto global (`window` / `globalThis`) | Objeto global |
| **Función** | Llamada directa `fn()` sin receptor | `undefined` | Objeto global |
| **Método** | `obj.metodo()` con receptor explícito | `obj` | `obj` |
| **Constructor** | `new Fn()` o `new Clase()` | Objeto recién creado | Objeto recién creado |
| **Explicit** | `fn.call(ctx)` / `fn.apply(ctx)` / `fn.bind(ctx)` | `ctx` | `ctx` |

Las arrow functions no aparecen en la tabla porque no tienen binding propio — heredan el `this` del cierre léxico en que se definen.

## Regla de prioridad

Cuando coinciden varios mecanismos de binding, el que gana es el de mayor rango:

```
new  >  explicit (call/apply/bind)  >  método (implicit)  >  default (función directa)
```

`new` puede crear un objeto ignorando el `this` fijado por `bind` (caso raro: función atada usada como constructora).

## Árbol de decisión

```
¿Se llama con new?          → this = objeto nuevo
¿Se llama con call/apply/bind? → this = primer argumento
¿Se llama como obj.metodo() → this = obj
¿Se llama directamente fn() → strict: undefined | non-strict: globalThis
¿Es arrow function?         → this del cierre léxico (ninguno de los anteriores aplica)
```

## Notas relacionadas

- [[01 this Global]]
- [[02 this en Función]]
- [[03 this en Método]]
- [[04 this en Constructor]]
- [[05 this en Arrow Functions]]
- [[06 Pérdida de this]]
- [[07 call()]]
- [[08 apply()]]
- [[09 bind()]]
- [[02 Prototipos/index|Prototipos]] — `this` en funciones constructoras prototípicas
- [[03 Clases/index|Clases]] — `this` dentro de clases y campos privados
