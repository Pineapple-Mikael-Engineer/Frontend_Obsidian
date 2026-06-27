---
title: Objetos Literales — mapa de propiedades clave → valor
aliases:
  - object literals
  - objetos literales JS
tags:
  - javascript
  - api/concepto
  - objetos
draft: false
order: 1
---

# Objetos Literales

> [!definicion]
> Un objeto literal es una expresión que crea un objeto en línea: una colección ordenada de **propiedades** (pares clave → valor) delimitada por llaves `{}`. Las claves son internamente strings o Symbols; los valores pueden ser cualquier expresión JavaScript. El objeto hereda de `Object.prototype` a través de la cadena de prototipos.

El modelo mental fundamental: un objeto es un **mapa mutable** de propiedades. Cada propiedad está descrita por un descriptor interno que controla si se puede leer, escribir, enumerar y configurar. La cadena de prototipos permite que propiedades no encontradas en el objeto propio se busquen en objetos ancestros.

```js
const punto = { x: 3, y: 4 };          // objeto literal mínimo
punto.z = 0;                            // añadir propiedad dinámicamente
console.log(Object.keys(punto));        // → ["x", "y", "z"]
```

## Nodos de esta sección

- [[01 Creación y Propiedades]] — sintaxis `{}`, tipos de valores, `Object.defineProperty`, descriptores
- [[02 Métodos]] — funciones como propiedades, `this`, method chaining, `toString`/`valueOf`
- [[03 Acceso (dot y bracket)]] — notación punto y corchete, `?.` optional chaining
- [[04 Propiedades Calculadas]] — claves dinámicas `[expresión]`, Symbols, recetas con `reduce`
- [[05 Shorthand de Propiedades y Métodos]] — azúcar sintáctico ES6, `super` en shorthand
- [[06 Getters y Setters]] — accessor properties, descriptores `get`/`set`, solo-lectura
- [[07 Recorrer Propiedades (keys, values, entries)]] — `Object.keys/values/entries`, `for...in`, Symbols
- [[08 Comprobar y Eliminar (in, hasOwnProperty, delete)]] — `in`, `Object.hasOwn`, `delete`, inmutabilidad

## Tabla comparativa de operaciones

| Operación | API / Sintaxis | Incluye heredadas | Incluye no-enumerables | Incluye Symbols |
|-----------|---------------|:-----------------:|:---------------------:|:---------------:|
| Claves propias enumerables | `Object.keys(obj)` | No | No | No |
| Valores propios enumerables | `Object.values(obj)` | No | No | No |
| Pares propios enumerables | `Object.entries(obj)` | No | No | No |
| Todas las propias (sin Symbols) | `Object.getOwnPropertyNames(obj)` | No | Sí | No |
| Symbols propios | `Object.getOwnPropertySymbols(obj)` | No | Sí | Sí |
| Enumerar propias + heredadas | `for...in` | Sí | No | No |
| Comprobar existencia (cadena) | `"prop" in obj` | Sí | Sí | Sí |
| Comprobar propiedad propia | `Object.hasOwn(obj, "prop")` | No | Sí | Sí |
| Eliminar propiedad | `delete obj.prop` | — | Solo configurables | — |
| Definir con descriptor | `Object.defineProperty(obj, prop, desc)` | — | — | — |

## Cómo funciona por dentro

Internamente cada objeto es una estructura de tipo **hash map** de slots de propiedad. Cada slot almacena un descriptor con los atributos `value`, `writable`, `enumerable`, `configurable` (propiedad de datos) o `get`, `set`, `enumerable`, `configurable` (propiedad de acceso). La cadena de prototipos se implementa como un puntero interno `[[Prototype]]` que el motor sigue cuando `[[Get]]` no encuentra la clave en el objeto propio.

## Notas relacionadas

- [[02 Prototipos/index | Prototipos]] — cadena de prototipos, herencia prototípica
- [[04 this/index | this]] — cómo se vincula `this` en métodos de objeto
- [[05 Encapsulación y Abstracción/index | Encapsulación]] — closures y módulos para datos privados
