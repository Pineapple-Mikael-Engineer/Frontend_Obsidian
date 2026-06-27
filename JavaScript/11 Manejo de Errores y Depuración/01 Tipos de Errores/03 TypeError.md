---
title: TypeError — valor no soporta la operación solicitada
aliases:
  - TypeError
  - type error js
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
order: 3
---

# TypeError

> [!definicion]
> `TypeError` se lanza cuando un valor existe pero no es del tipo que la operación requiere: llamar algo que no es función, acceder a una propiedad de `null` o `undefined`, usar un primitivo como constructor, o reasignar una constante sellada. Es el error de producción más frecuente en JavaScript.

```js
const usuario = null;

try {
  console.log(usuario.nombre);
} catch (err) {
  console.log(err instanceof TypeError); // → true
  console.log(err.message);
  // → "Cannot read properties of null (reading 'nombre')"
}
```

**`name`:** `"TypeError"`.

## Causas más comunes

| Causa | Ejemplo | Mensaje de error |
|-------|---------|-----------------|
| Acceder a prop de `null` | `null.foo` | `Cannot read properties of null (reading 'foo')` |
| Acceder a prop de `undefined` | `undefined.foo` | `Cannot read properties of undefined (reading 'foo')` |
| Llamar a `null`/`undefined` como función | `null()` | `null is not a function` |
| Llamar a un método inexistente | `[].foo()` | `[].foo is not a function` |
| `new` sobre un primitivo | `new 42` | `42 is not a constructor` |
| Reasignar `const` en módulo | `const X = 1; X = 2` | `Assignment to constant variable` |
| Operación ilegal en strict mode | `delete Object.prototype` | `Cannot delete property` |

## El error de producción más frecuente

`"Cannot read properties of undefined (reading 'X')"` aparece casi siempre cuando una respuesta de API llega vacía, un estado de React no se inicializa, o una referencia al DOM se consulta antes de montarse.

```js
// Patrón problemático:
function mostrarNombre(usuario) {
  return usuario.perfil.nombre; // TypeError si perfil es undefined
}

// Guard con optional chaining:
function mostrarNombre(usuario) {
  return usuario?.perfil?.nombre ?? "Sin nombre";
}

// Guard con try/catch (útil cuando el error es inesperado en todo el bloque):
function cargarDatos(raw) {
  try {
    return raw.items.map(item => item.id);
  } catch (err) {
    if (err instanceof TypeError) {
      console.error("Estructura inesperada:", raw);
      return [];
    }
    throw err;
  }
}
```

## Optional chaining vs try/catch para TypeError

| Estrategia | Cuándo usarla |
|-----------|--------------|
| `?.` optional chaining | Acceso a propiedades anidadas de objetos que pueden ser null/undefined |
| `??` nullish coalescing | Proveer un valor por defecto cuando el resultado es null/undefined |
| `try/catch` | Bloque entero de lógica de dominio donde cualquier paso puede fallar |
| `typeof x === "function"` antes de llamar | Callbacks opcionales, plugins, feature detection |

Combinar `?.` con `??` resuelve la mayoría de `TypeError` por acceso a propiedades sin necesidad de try/catch.

## Strict mode y TypeError adicionales

Algunas operaciones que en sloppy mode fallan silenciosamente lanzan `TypeError` en strict mode:

```js
"use strict";

// Escribir en propiedad de solo lectura:
const obj = Object.freeze({ x: 1 });
obj.x = 2; // → TypeError: Cannot assign to read only property 'x'

// Duplicar parámetros:
// function f(a, a) {} → SyntaxError en strict mode (no TypeError)

// Llamar a getter sin setter:
const o = { get val() { return 1; } };
o.val = 5; // → TypeError en strict mode
```

> [!tip]
> Al depurar `TypeError` en la consola del navegador, el mensaje en Chrome/V8 incluye el nombre de la propiedad entre paréntesis: `(reading 'nombre')`. Ese fragmento identifica exactamente qué propiedad se intentaba leer, lo que indica cuál es el objeto nulo en la cadena de acceso.

> [!warning]
> `"x is not a function"` no siempre significa que `x` sea `null`. Puede ser que `x` exista pero sea un string, número u objeto. Verificar el tipo real con `typeof x` o inspeccionando en el debugger antes de asumir que es `null`.

## Notas relacionadas

- [[01 Tipos de Errores/index | Tipos de Errores]]
- [[01 Tipos de Errores/02 ReferenceError | ReferenceError]]
- [[02 try - catch - finally/01 try y catch | try y catch]]
