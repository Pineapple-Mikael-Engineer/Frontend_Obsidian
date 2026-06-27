---
title: Tipos de Datos — Visión general del sistema de tipos de JavaScript
aliases:
  - tipos de datos JS
  - type system JavaScript
tags:
  - javascript
  - api/concepto
  - tipos
objeto: global
tipo: concepto
draft: false
order: 3
---

# Tipos de Datos

> [!definicion]
> JavaScript tiene **8 tipos de datos**: 7 **primitivos** (number, string, boolean, undefined, null, symbol, bigint) y el tipo **object**. El lenguaje es de **tipado dinámico**: una variable no tiene tipo fijo; el tipo lo tiene el valor almacenado en ella, y puede cambiar en tiempo de ejecución.

La distinción fundamental es **primitivo vs. objeto**:

- **Primitivos** — valores inmutables pasados **por valor**. Una copia independiente en cada asignación.
- **Objetos** — valores mutables pasados **por referencia**. Asignar o pasar un objeto comparte el mismo dato en memoria.

```js
// Por valor (primitivos)
let a = 42;
let b = a;
b = 99;
console.log(a); // → 42  (a no cambia)

// Por referencia (objetos)
let obj1 = { x: 1 };
let obj2 = obj1;
obj2.x = 99;
console.log(obj1.x); // → 99  (mismo objeto en memoria)
```

## Tipado dinámico

Una variable puede contener valores de tipos distintos en distintos momentos. El tipo se determina en tiempo de ejecución con `typeof`.

```js
let dato = 42;
typeof dato; // → "number"

dato = "hola";
typeof dato; // → "string"

dato = null;
typeof dato; // → "object"  ← bug histórico, ver nota typeof
```

## Los 8 tipos

| Tipo | Categoría | `typeof` | Ejemplo |
|---|---|---|---|
| `number` | primitivo | `"number"` | `3.14`, `NaN`, `Infinity` |
| `string` | primitivo | `"string"` | `"hola"`, `` `texto` `` |
| `boolean` | primitivo | `"boolean"` | `true`, `false` |
| `undefined` | primitivo | `"undefined"` | variable sin inicializar |
| `null` | primitivo | `"object"` | ausencia intencional de objeto |
| `symbol` | primitivo | `"symbol"` | `Symbol("id")` |
| `bigint` | primitivo | `"bigint"` | `9007199254740991n` |
| `object` | objeto | `"object"` | `{}`, `[]`, `new Date()` |

`typeof` no distingue `null` de un objeto real ni un array de un objeto plano. Para esos casos: [[02 typeof|typeof]] y [[03 instanceof|instanceof]].

## Primitivos e inmutabilidad

Los primitivos son **inmutables**: no se puede cambiar el valor en su lugar. Una operación sobre un string no modifica el original; produce un string nuevo.

```js
let s = "hola";
s.toUpperCase(); // → "HOLA"
console.log(s);  // → "hola"  (s no cambió)
```

El auto-boxing (envolver el primitivo en su objeto wrapper `String`, `Number`, `Boolean`) ocurre automáticamente para acceder a los métodos del prototipo, y se descarta de inmediato.

## Subsecciones

- [[01 Primitivos/index|Primitivos]] — los 7 tipos primitivos, inmutabilidad, auto-boxing.
- [[04 Conversión de Tipos/index|Conversión de Tipos]] — coerción implícita y explícita, truthy/falsy, igualdad.

## Operadores de introspección de tipo

- [[02 typeof]] — devuelve un string con el tipo; útil para primitivos y para variables potencialmente no declaradas.
- [[03 instanceof]] — verifica la cadena de prototipos; útil para objetos construidos con `new`.

## Notas relacionadas

- [[01 Primitivos/index|Primitivos]]
- [[04 Conversión de Tipos/index|Conversión de Tipos]]
- [[02 typeof]]
- [[03 instanceof]]
