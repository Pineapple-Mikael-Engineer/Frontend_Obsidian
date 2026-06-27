---
title: Object.assign() — Copiar propiedades entre objetos
aliases:
  - Object assign
  - assign
tags:
  - javascript
  - api/funcion
  - funcional
draft: false
order: 2
---

# Object.assign()

> [!definicion]
> `Object.assign(destino, ...fuentes)` copia las **propiedades propias enumerables** (incluyendo Symbols enumerables propios) de todas las fuentes al objeto destino, en orden de izquierda a derecha. **Muta y devuelve el destino.** Para clonar sin mutar, el destino debe ser un objeto literal nuevo: `Object.assign({}, fuente)`.

```js
const destino = { a: 1 };
const fuente = { b: 2, c: 3 };

Object.assign(destino, fuente);
// destino es ahora { a: 1, b: 2, c: 3 }
// fuente no cambia
```

## Firma

| Parámetro | Tipo | Descripción |
|---|---|---|
| `destino` | object | Objeto receptor; se muta y se devuelve |
| `...fuentes` | object (variadic) | Uno o más objetos fuente; se leen en orden |

| Aspecto | Detalle |
|---|---|
| Valor de retorno | El propio objeto `destino` (mutado) |
| Muta el destino | Sí |
| Muta las fuentes | No |
| Propiedades no enumerables | No se copian |
| Propiedades del prototipo | No se copian |
| Symbols propios enumerables | Sí se copian |

## Clonar sin mutar (patrón correcto)

```js
const original = { nombre: "Ana", edad: 30 };
const copia = Object.assign({}, original);

copia.edad = 31;
console.log(original.edad); // 30 — el original no se modificó
```

El primer argumento `{}` es el objeto nuevo que actúa de destino; las propiedades de `original` se copian ahí.

## Fusionar múltiples fuentes

Las fuentes se procesan en orden. Si hay propiedades duplicadas, la fuente más a la derecha sobreescribe.

```js
const DEFAULTS = { timeout: 3000, reintentos: 3, verbose: false };
const opcionesUsuario = { timeout: 5000, verbose: true };

const config = Object.assign({}, DEFAULTS, opcionesUsuario);
// { timeout: 5000, reintentos: 3, verbose: true }
```

Este patrón de defaults es una receta clásica anterior a ES2015. Con spread es equivalente: `{ ...DEFAULTS, ...opcionesUsuario }`.

## Diferencias con spread `{ ...obj }`

| Aspecto | `Object.assign(dest, src)` | `{ ...src }` |
|---|---|---|
| Activa setters del destino | Sí — usa el slot interno Set | No — crea propiedades directamente |
| Symbols propios enumerables | Sí | Sí |
| Propiedades no enumerables | No | No |
| Propiedades del prototipo | No | No |
| Muta el destino | Sí (si no es `{}` nuevo) | No aplica (siempre crea objeto nuevo) |
| Disponibilidad | ES2015 | ES2018 para objetos |

El comportamiento de los **setters** es la diferencia clave: si el objeto destino tiene una propiedad definida con un setter, `Object.assign` lo invocará, mientras que spread crea la propiedad directamente sin invocar el setter.

```js
const obj = {
  _x: 0,
  set x(val) { this._x = val * 2; }
};

Object.assign(obj, { x: 5 });
console.log(obj._x); // 10 — setter activado

const obj2 = { ...obj, x: 5 };
// obj2 no tiene setter, la propiedad x se crea directamente con valor 5
```

## Cómo funciona por dentro

`Object.assign` itera cada fuente con `[[OwnPropertyKeys]]` (que devuelve strings y Symbols propios). Para cada clave cuya propiedad sea enumerable, llama a `[[Get]]` en la fuente para obtener el valor y luego a `[[Set]]` en el destino para asignarlo. Este uso de `[[Set]]` es lo que activa los setters.

No copia descriptores completos (writable, enumerable, configurable): las propiedades copiadas siempre son propiedades de datos normales en el destino, no getters/setters. Para copiar descriptores completos, usar `Object.defineProperties` junto a `Object.getOwnPropertyDescriptors`.

```js
// Copiar con descriptores completos (incluyendo getters/setters)
const src = {
  get nombre() { return "Ana"; }
};
const dest = Object.defineProperties({}, Object.getOwnPropertyDescriptors(src));
// dest.nombre actúa como getter, no es "Ana" como valor estático
```

> [!tip] Buenas prácticas
> - Para clonar objetos planos en código moderno, preferir spread `{ ...obj }` por su sintaxis más concisa.
> - Usar `Object.assign` cuando se necesite activar setters del destino explícitamente, o al trabajar con código ES5-compatible.
> - El patrón `Object.assign({}, DEFAULTS, opciones)` sigue siendo útil para documentar claramente la intención de "configuración con defaults".
> - Para copias profundas, usar `structuredClone()` (nativo, ES2022) en lugar de encadenar `Object.assign`.

> [!warning] Errores comunes
> - **Mutar el primer argumento sin querer:** `Object.assign(original, cambios)` modifica `original`. Si no es la intención, el primer argumento debe ser `{}`.
> - **Asumir deep copy:** objetos anidados se copian como referencia. Mutar el anidado en la copia afecta al original.
> - **No copiar propiedades del prototipo:** si `fuente` hereda propiedades de su prototipo, `Object.assign` no las copia. Solo propiedades propias.
> - **Confundir retorno con nueva copia:** `Object.assign` devuelve el mismo objeto `destino`, no uno nuevo. `const x = Object.assign(destino, src)` hace que `x === destino`.

## Notas relacionadas

- [[index|Inmutabilidad Práctica — Índice]]
- [[01 Spread Operator]]
- [[03 Array.from()]]
