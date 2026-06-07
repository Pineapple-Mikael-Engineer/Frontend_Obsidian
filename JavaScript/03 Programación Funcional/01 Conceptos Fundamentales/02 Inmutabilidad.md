---
title: Inmutabilidad — Producir nuevos valores en lugar de mutar
aliases:
  - inmutabilidad
  - immutability
  - inmutable
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
---

# Inmutabilidad

> [!definicion]
> La **inmutabilidad** es el principio por el que un valor no se modifica una vez creado — en su lugar, cualquier "cambio" produce un **nuevo valor** que refleja el estado deseado. Los primitivos de JS (números, strings, booleans, `null`, `undefined`, `Symbol`, `BigInt`) son inmutables por naturaleza del lenguaje; los objetos y arrays son mutables por defecto y la inmutabilidad hay que imponerla explícitamente.

```js
// Los strings son inmutables: los métodos retornan nuevos strings
const saludo = "hola";
const mayus = saludo.toUpperCase(); // → "HOLA"
console.log(saludo); // → "hola"  — el original no cambia

// Los arrays son mutables: hay que evitar los métodos que mutan
const nums = [3, 1, 2];
const ordenado = [...nums].sort(); // copia primero, luego ordena
console.log(nums);    // → [3, 1, 2]  — intacto
console.log(ordenado); // → [1, 2, 3]  — nuevo array
```

## Primitivos vs objetos: distinto comportamiento

| Tipo | Mutable | Ejemplo de mutación |
|---|---|---|
| `number` | no | `let x = 5; x = 6` reasigna binding, no muta el valor |
| `string` | no | `str[0] = "X"` silently falla (no hace nada) |
| `boolean` | no | igual que number |
| `object` | sí | `obj.prop = valor` muta el objeto en el heap |
| `array` | sí | `arr.push(x)` muta el array en el heap |
| `Map` / `Set` | sí | `.set()`, `.add()` mutan la colección |

## Técnicas para arrays inmutables

Los métodos que crean un array nuevo son las herramientas principales. Los que mutan el original son `push`, `pop`, `shift`, `unshift`, `splice`, `sort`, `reverse`, `fill`, `copyWithin`.

```js
const frutas = ["manzana", "pera", "uva"];

// Agregar al final — inmutable
const conNaranja = [...frutas, "naranja"];

// Agregar al principio
const conFresa = ["fresa", ...frutas];

// Eliminar por índice (índice 1)
const sinPera = frutas.filter((_, i) => i !== 1);

// Actualizar por índice
const actualizado = frutas.map((f, i) => i === 2 ? "kiwi" : f);

// Concatenar dos arrays
const mas = frutas.concat(["mango", "piña"]);
```

### Métodos `to*` de ES2023 — la solución nativa

ES2023 añade versiones no mutantes de los cuatro métodos que históricamente mutaban el array:

```js
const nums = [3, 1, 4, 1, 5];

nums.toSorted();         // → [1, 1, 3, 4, 5] — nuevo array
nums.toReversed();       // → [5, 1, 4, 1, 3] — nuevo array
nums.toSpliced(2, 1);    // → [3, 1, 1, 5]    — elimina índice 2, nuevo array
nums.with(0, 99);        // → [99, 1, 4, 1, 5] — reemplaza índice 0, nuevo array

console.log(nums); // → [3, 1, 4, 1, 5]  — original intacto
```

## Técnicas para objetos inmutables

```js
const usuario = { nombre: "Ana", edad: 30, activo: true };

// Actualizar una propiedad — spread crea objeto nuevo
const conNombreNuevo = { ...usuario, nombre: "Beatriz" };

// Agregar una propiedad
const conRol = { ...usuario, rol: "admin" };

// Eliminar una propiedad — destructuring con rest
const { activo, ...sinActivo } = usuario;

// Object.assign — equivalente al spread (shallow)
const copia = Object.assign({}, usuario, { edad: 31 });
```

## `Object.freeze` — congelación superficial

`Object.freeze(obj)` hace que las propiedades de **primer nivel** sean no-escribibles y no-configurables. Es **superficial**: los objetos anidados siguen siendo mutables.

```js
const config = Object.freeze({
  host: "localhost",
  db: { port: 5432 },
});

config.host = "produccion"; // silently falla en sloppy mode; TypeError en strict mode
console.log(config.host); // → "localhost"  — no cambió

config.db.port = 9999; // ← sí funciona: db es un objeto anidado, no está congelado
console.log(config.db.port); // → 9999  — mutó
```

### `Object.freeze` profundo — hay que recorrer el árbol

```js
const deepFreeze = obj => {
  Object.keys(obj).forEach(key => {
    if (typeof obj[key] === "object" && obj[key] !== null) {
      deepFreeze(obj[key]);
    }
  });
  return Object.freeze(obj);
};

const config = deepFreeze({ host: "localhost", db: { port: 5432 } });
config.db.port = 9999; // TypeError en strict mode
```

## Recetas comunes

### Estado en Redux / zustand — nunca mutar el estado anterior

```js
// Reducer puro: retorna un nuevo objeto, nunca muta state
const reducer = (state, action) => {
  switch (action.type) {
    case "INCREMENTAR":
      return { ...state, contador: state.contador + 1 };
    case "AGREGAR_ITEM":
      return { ...state, items: [...state.items, action.payload] };
    default:
      return state;
  }
};
```

### Historial de estados (undo/redo)

```js
// Porque cada estado es un nuevo objeto, mantener historial es trivial
let historial = [estadoInicial];
let cursor = 0;

const aplicar = (estado, accion) => {
  const nuevoEstado = reducer(estado, accion);
  historial = [...historial.slice(0, cursor + 1), nuevoEstado];
  cursor++;
  return nuevoEstado;
};

const deshacer = () => { cursor--; return historial[cursor]; };
const rehacer = () => { cursor++; return historial[cursor]; };
```

## Cómo funciona por dentro

El spread `{ ...obj }` llama internamente a `Object.assign({}, obj)`: itera las **propiedades propias enumerables** del objeto fuente y las copia al destino. El resultado es una **copia superficial** (shallow copy): los valores primitivos se copian por valor; los valores que son referencias (objetos, arrays anidados) se copian como referencia — ambos objetos apuntan al mismo objeto interno en el heap.

```js
const a = { x: 1, nested: { y: 2 } };
const b = { ...a };

b.x = 99;        // no afecta a a.x
b.nested.y = 99; // sí afecta a a.nested.y — misma referencia
console.log(a);  // → { x: 1, nested: { y: 99 } }
```

Para una copia verdaderamente profunda usar `structuredClone(obj)` (nativo desde Node 17 y navegadores modernos 2022) — clona recursivamente pero no preserva `undefined`, funciones ni `Symbol`.

> [!tip] Buenas prácticas
> - Preferir `const` para todos los bindings de objetos — no impide mutación pero señala la intención de no reasignar.
> - Usar `toSorted`, `toReversed`, `with`, `toSpliced` en lugar de sus contrapartes mutantes siempre que el soporte lo permita (baseline 2023).
> - Para objetos de configuración, aplicar `Object.freeze` — previene mutaciones accidentales en runtime.
> - Para colecciones grandes con muchas actualizaciones, evaluar Immer (produce un borrador mutable internamente y devuelve un objeto inmutable) o Immutable.js (estructuras de datos persistentes con complejidad `O(log n)` en actualizaciones).

> [!warning] Errores comunes
> - **Confundir `const` con inmutabilidad**: `const arr = []` solo impide reasignar el binding; `arr.push(x)` muta el array sin error.
> - **Spread superficial en objetos anidados**: `{ ...obj }` no protege las propiedades anidadas — si se modifica `copia.nested.x` se modifica también `original.nested.x`.
> - **`Object.freeze` y fallar silenciosamente**: en sloppy mode, intentar mutar un objeto congelado no lanza error — el cambio simplemente no ocurre, lo que produce bugs difíciles de rastrear. Activar `"use strict"` para que lance `TypeError`.
> - **`sort` y `reverse` mutan el original**: `arr.sort()` ordena `arr` in-place. Siempre usar `[...arr].sort()` o `arr.toSorted()`.

## Notas relacionadas

- [[01 Funciones Puras]] — la inmutabilidad es la técnica que hace posible la pureza con objetos.
- [[03 Efectos Secundarios]] — mutar un argumento es el efecto secundario más frecuente.
- [[05 Declarativo vs Imperativo]] — el estilo declarativo asume datos inmutables para componer transformaciones.
