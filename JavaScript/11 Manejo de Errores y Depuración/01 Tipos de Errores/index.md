---
title: Tipos de Errores — mapa de la sección
aliases:
  - error types JS
  - tipos de error javascript
tags:
  - javascript
  - api/concepto
  - errores
objeto: Error
tipo: concepto
retorna: "-"
muta: false
asincrono: false
draft: false
---

# Tipos de Errores

> [!definicion]
> Todo error en tiempo de ejecución de JavaScript es una instancia de `Error` o de alguna de sus subclases nativas. El motor crea el objeto, completa sus propiedades y lo lanza; el programa puede capturarlo con `try/catch` o dejarlo propagarse hasta el host.

Las tres propiedades comunes a todos los errores nativos:

| Propiedad | Tipo | Descripción |
|-----------|------|-------------|
| `name` | `string` | Nombre del constructor (`"TypeError"`, `"RangeError"`, …) |
| `message` | `string` | Texto descriptivo, legible por humanos |
| `stack` | `string` | Traza de llamadas completa (no estándar, pero soportada en todos los engines modernos) |

```js
const err = new TypeError("x no es una función");
console.log(err.name);    // → "TypeError"
console.log(err.message); // → "x no es una función"
console.log(err.stack);   // → "TypeError: x no es una función\n    at <anonymous>:1:13"
```

## Los seis tipos nativos

| Tipo | Cuándo se lanza | Ejemplo de mensaje |
|------|-----------------|--------------------|
| `SyntaxError` | El parser encuentra sintaxis inválida | `Unexpected token '}'` |
| `ReferenceError` | Se accede a una variable que no existe en el scope | `x is not defined` |
| `TypeError` | El valor no soporta la operación solicitada | `Cannot read properties of null` |
| `RangeError` | Un valor cae fuera del rango permitido | `Maximum call stack size exceeded` |
| `URIError` | `decodeURIComponent`/`decodeURI` recibe secuencia inválida | `URI malformed` |
| `EvalError` | Históricamente ligado a `eval()`; raro en engines modernos | — |

## Jerarquía de herencia

```
Error
├── SyntaxError
├── ReferenceError
├── TypeError
├── RangeError
├── URIError
└── EvalError
```

Todos comparten la cadena de prototipos: `TypeError.prototype → Error.prototype → Object.prototype`.

## Discriminar el tipo en catch

```js
try {
  JSON.parse("{mal json}");
} catch (err) {
  if (err instanceof SyntaxError) {
    console.log("JSON inválido:", err.message);
  } else {
    throw err; // relanzar lo que no se maneja
  }
}
```

`instanceof` es más robusto que comparar `err.name` como string porque sobrevive a errores personalizados que extienden la clase nativa.

## Crear errores personalizados

```js
class ValidationError extends Error {
  constructor(message, field) {
    super(message);
    this.name = "ValidationError";
    this.field = field;
  }
}

throw new ValidationError("Campo requerido", "email");
// ValidationError: Campo requerido
```

El patrón `extends Error` con `super(message)` y asignación manual de `this.name` es el estándar. Se delega el detalle a [[03 throw/index | 03 throw]].

## Notas relacionadas

- [[01 Tipos de Errores/01 SyntaxError | SyntaxError]]
- [[01 Tipos de Errores/02 ReferenceError | ReferenceError]]
- [[01 Tipos de Errores/03 TypeError | TypeError]]
- [[01 Tipos de Errores/04 RangeError | RangeError]]
- [[01 Tipos de Errores/05 Otros (URIError, EvalError) | URIError y EvalError]]
- [[02 try - catch - finally/index | try / catch / finally]]
- [[03 throw/index | throw y errores personalizados]]
