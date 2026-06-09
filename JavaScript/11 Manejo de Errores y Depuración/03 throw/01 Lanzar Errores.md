---
title: Lanzar Errores
aliases:
  - throw statement
  - lanzar error js
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

# Lanzar Errores

> [!definicion]
> `throw expresión` evalúa la expresión y la lanza como error, interrumpiendo la ejecución del bloque actual. El valor sube por el call stack hasta ser capturado por un `catch` o, si no lo hay, termina el programa con un `Uncaught Error`. Se puede lanzar cualquier valor JS, pero solo los objetos `Error` (o subclases) aportan `stack trace` automático.

## Propiedades del objeto Error

| Propiedad | Estándar | Descripción |
|-----------|----------|-------------|
| `name` | Sí | Nombre del constructor: `"Error"`, `"TypeError"`, etc. |
| `message` | Sí | Texto descriptivo pasado al constructor |
| `stack` | No (universal) | Cadena con la traza de llamadas al momento de creación |
| `cause` | ES2022 | Error original que originó este (encadenamiento) |

```js
const err = new Error("fallo de red", { cause: originalErr });
console.log(err.message); // → "fallo de red"
console.log(err.cause);   // → originalErr
console.log(err.stack);
// Error: fallo de red
//     at fetchDatos (<anonymous>:3:9)
//     at main (<anonymous>:10:3)
```

## Por qué solo lanzar objetos Error

Un `throw "mensaje"` o `throw 42` funciona sintácticamente, pero:

- no genera `stack trace` — imposible saber de dónde vino el error.
- rompe el patrón `err.message` que el resto del código (logging, monitoring) espera.
- impide usar `instanceof` para discriminar el tipo.

Los objetos `Error` nativos registran el stack en el momento de construcción (`new Error(...)`), no en el momento del `throw` — crear el error tarde puede perder contexto.

## Tipos nativos instanciables directamente

```js
new Error("descripción genérica")
new TypeError("se esperaba un número")
new RangeError("índice fuera de rango: -1")
new ReferenceError("variable no definida")
new SyntaxError("JSON inválido")
new URIError("secuencia URI malformada")
```

La elección del tipo comunica semántica: un `TypeError` en validación de argumentos es más informativo que un `Error` genérico. Se delega el detalle de los tipos nativos a [[01 Tipos de Errores/index | Tipos de Errores]].

## Re-lanzar errores

El patrón correcto en un `catch` que no puede manejar todos los errores: comprobar tipo y relanzar lo desconocido.

```js
try {
  operacion();
} catch (err) {
  if (err instanceof NetworkError) {
    mostrarMensajeConexion();
  } else {
    throw err; // re-lanzar — no se sabe manejar
  }
}
```

Relanzar sin `throw err` (tragarse el error) es uno de los bugs más comunes: el programa continúa en un estado inválido silenciosamente.

## Encadenar errores con `error.cause`

`cause` (ES2022) preserva el error original al envolver con contexto adicional:

```js
async function cargarUsuario(id) {
  try {
    return await fetch(`/api/users/${id}`);
  } catch (err) {
    throw new Error(`No se pudo cargar el usuario ${id}`, { cause: err });
  }
}
```

Al inspeccionar el error de alto nivel, `err.cause` contiene el error de red original con su propio `stack`. Evita perder información al escalar errores entre capas.

> [!tip]
> Construir el objeto `Error` con `new` en el punto donde se detecta el problema — no reutilizar instancias. El `stack` se captura al construir, no al lanzar.

> [!warning]
> `error.cause` solo existe en ES2022+. En entornos más antiguos (Node < 16.9, Safari < 15.4), el segundo argumento del constructor `Error` se ignora sin error — `cause` quedará `undefined`.

## Notas relacionadas

- [[03 throw/02 Errores Personalizados (extends Error) | Errores Personalizados]]
- [[02 try - catch - finally/index | try / catch / finally]]
- [[01 Tipos de Errores/index | Tipos de Errores nativos]]
