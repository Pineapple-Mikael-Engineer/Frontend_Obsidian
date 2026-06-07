---
title: typeof — Operador de introspección de tipo
aliases:
  - typeof JS
  - operador typeof
tags:
  - javascript
  - api/operador
  - tipos
objeto: global
tipo: operador
retorna: string
muta: false
asincrono: false
draft: false
---

# typeof

> [!definicion]
> `typeof` es un operador unario que retorna un **string** que describe el tipo del operando. No evalúa el valor del operando sino su tipo de representación. Es el mecanismo principal para introspección de tipos en JavaScript, con la salvedad de que `typeof null === "object"` es un bug histórico no corregible.

```js
typeof 42          // → "number"
typeof "hola"      // → "string"
typeof true        // → "boolean"
typeof undefined   // → "undefined"
typeof null        // → "object"   ← bug histórico
typeof {}          // → "object"
typeof []          // → "object"   (los arrays son objetos)
typeof function(){} // → "function"
typeof Symbol()    // → "symbol"
typeof 42n         // → "bigint"
```

## Tabla completa de resultados

| Operando | Resultado de `typeof` |
|---|---|
| `number` (incluye `NaN`, `Infinity`) | `"number"` |
| `string` | `"string"` |
| `boolean` | `"boolean"` |
| `undefined` | `"undefined"` |
| `null` | `"object"` |
| objeto plano `{}` | `"object"` |
| array `[]` | `"object"` |
| `Date`, `RegExp`, etc. | `"object"` |
| función declarada o expresion | `"function"` |
| `Symbol()` | `"symbol"` |
| `bigint` literal `n` | `"bigint"` |

Las funciones (incluyendo clases) retornan `"function"` aunque técnicamente sean objetos. Todos los demás objetos (arrays, dates, regexp, mapas) retornan `"object"`.

## El bug de `typeof null`

```js
typeof null   // → "object"
```

La representación interna original de 1995 usaba etiquetas de 3 bits para el tipo: `000` = objeto. El puntero nulo tenía sus bits iniciales en cero y fue clasificado como objeto. El bug permanece porque corregirlo rompería millones de sitios.

Para comprobar null correctamente:

```js
valor === null    // único modo fiable
```

## Ventaja: seguridad con variables no declaradas

`typeof` es el único operador que no lanza `ReferenceError` cuando se aplica a una variable no declarada:

```js
// Si 'foo' no está declarada:
typeof foo    // → "undefined"  (no lanza error)
console.log(foo)  // → ReferenceError

// Patrón clásico para detección de entorno
if (typeof window !== "undefined") { /* navegador */ }
if (typeof process !== "undefined") { /* Node.js */ }
```

> [!warning] TDZ y `typeof`
> Para `let` y `const` en su Temporal Dead Zone (antes de la línea de declaración dentro del bloque), `typeof` sí lanza `ReferenceError`. La excepción de "no lanza error" aplica solo a variables completamente no declaradas.
> ```js
> {
>   typeof x;   // → ReferenceError (TDZ)
>   let x = 1;
> }
> ```

## `typeof` vs `instanceof` para objetos

`typeof` solo distingue `"function"` de `"object"` entre los tipos de referencia. Para saber si un objeto es de una clase específica, se usa [[03 instanceof|instanceof]]:

```js
typeof []         // → "object"  (no dice que es array)
[] instanceof Array   // → true

typeof new Date() // → "object"
new Date() instanceof Date   // → true
```

La limitación de `instanceof` es que falla en contextos cross-realm (distintos iframes).

## Recetas comunes

```js
// Detectar si una variable es un número válido (no NaN)
typeof x === "number" && !Number.isNaN(x)

// Guard para función opcional
if (typeof callback === "function") callback(resultado);

// Detectar si es un tipo primitivo
["number","string","boolean","symbol","bigint"].includes(typeof x)

// Distinguir array de objeto plano (typeof no sirve: ambos → "object")
Array.isArray(valor)      // → true solo para arrays

// Detectar tipo interno real de cualquier valor
Object.prototype.toString.call([])      // → "[object Array]"
Object.prototype.toString.call(null)    // → "[object Null]"
Object.prototype.toString.call(/re/)    // → "[object RegExp]"
```

## Cómo funciona por dentro

`typeof` es un operador definido en la especificación ECMAScript que inspecciona el **tipo interno** (`[[Type]]`) del valor en la representación del motor, no el prototipo. Por eso funciona antes de que el value esté completamente inicializado para variables no declaradas: la búsqueda en el entorno de variables retorna la etiqueta "no declarada" en lugar de lanzar error de referencia.

> [!tip] Buenas prácticas
> - Usar `typeof x === "function"` para verificar callbacks antes de invocarlos.
> - Para arrays, usar `Array.isArray(x)`, no `typeof`.
> - Para `null`, usar `x === null`, no `typeof`.
> - Para detección de entorno (browser/Node), `typeof window !== "undefined"` es el patrón más seguro.

> [!warning] Errores comunes
> - `typeof null === "object"`: no indica que `null` sea un objeto; comprobar con `=== null`.
> - `typeof []` y `typeof {}` ambos retornan `"object"`: `typeof` no distingue arrays de objetos planos.
> - `typeof NaN === "number"`: `NaN` es de tipo number aunque no sea un número válido.

## Notas relacionadas

- [[03 instanceof|instanceof]] — verificar clase/prototipo de objetos
- [[05 null|null]] — el bug de `typeof null`
- [[04 undefined|undefined]] — `typeof` seguro con variables no declaradas
- [[03 Tipos de Datos/index|Tipos de Datos]]
