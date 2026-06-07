---
title: Propiedades Calculadas — claves dinámicas en objeto literal
aliases:
  - computed property names
  - claves dinámicas
  - computed properties
tags:
  - javascript
  - api/concepto
  - objetos
objeto: Object
tipo: concepto
draft: false
---

# Propiedades Calculadas

> [!definicion]
> La sintaxis de **propiedad calculada** `{ [expresión]: valor }` permite usar el resultado de una expresión como nombre de propiedad en un literal de objeto. La expresión entre corchetes se evalúa en tiempo de creación del objeto; el resultado se convierte a string (o se mantiene como Symbol si ya es un Symbol) y se usa como clave.

```js
const campo = "nombre";
const usuario = {
  [campo]: "Ana",           // equivale a { nombre: "Ana" }
  [`prefijo_${campo}`]: 42, // equivale a { prefijo_nombre: 42 }
};

console.log(usuario.nombre);          // → "Ana"
console.log(usuario.prefijo_nombre);  // → 42
```

## Casos de uso

### Claves desde variables o expresiones

```js
const ACCION = "submit";
const handlers = {
  [ACCION]: (e) => e.preventDefault(),
  [`on_${ACCION}`]: () => console.log("enviado"),
};
```

### Dispatch table — mapeo de constantes a funciones

```js
const SUMAR    = "SUMAR";
const RESTAR   = "RESTAR";
const DUPLICAR = "DUPLICAR";

const operaciones = {
  [SUMAR]:    (a, b) => a + b,
  [RESTAR]:   (a, b) => a - b,
  [DUPLICAR]: (a) => a * 2,
};

const resultado = operaciones[SUMAR](3, 4);
console.log(resultado); // → 7
```

El patrón evita cadenas de `if/else` o `switch` al mapear claves de acción directamente a sus implementaciones.

### Con Symbols — definir protocolos

Los Symbols bien conocidos (well-known Symbols) permiten implementar protocolos del lenguaje directamente en el literal:

```js
const rango = {
  inicio: 1,
  fin: 5,
  [Symbol.iterator]() {     // define el protocolo iterador
    let actual = this.inicio;
    const fin = this.fin;
    return {
      next() {
        return actual <= fin
          ? { value: actual++, done: false }
          : { value: undefined, done: true };
      },
    };
  },
};

console.log([...rango]); // → [1, 2, 3, 4, 5]

for (const n of rango) {
  process.stdout.write(n + " "); // → 1 2 3 4 5
}
```

## Recetas

### Crear objeto desde lista de claves con valor uniforme

```js
const permisos = ["leer", "escribir", "borrar"];
const roles = permisos.reduce((acc, p) => ({ ...acc, [p]: true }), {});
// → { leer: true, escribir: true, borrar: true }
```

### Crear objeto desde array de pares

```js
const pares = [["a", 1], ["b", 2], ["c", 3]];
const obj = Object.fromEntries(pares);
// equivalente a pares.reduce((acc, [k, v]) => ({ ...acc, [k]: v }), {})
console.log(obj); // → { a: 1, b: 2, c: 3 }
```

### Indexar colección por campo clave

```js
const usuarios = [
  { id: 1, nombre: "Ana" },
  { id: 2, nombre: "Luis" },
  { id: 3, nombre: "María" },
];

const porId = usuarios.reduce((acc, u) => ({ ...acc, [u.id]: u }), {});
console.log(porId[2].nombre); // → "Luis"
```

### Transformar estado con clave dinámica

```js
function actualizarCampo(estado, campo, valor) {
  return { ...estado, [campo]: valor }; // inmutable
}

const inicial = { nombre: "Ana", edad: 30 };
const nuevo = actualizarCampo(inicial, "edad", 31);
console.log(nuevo); // → { nombre: "Ana", edad: 31 }
```

## Cómo funciona por dentro

El parser transforma `{ [expr]: val }` en código que, al ejecutarse:

1. Evalúa `expr` en el contexto actual.
2. Si el resultado no es un Symbol, aplica `ToString(resultado)` para obtener la clave.
3. Si el resultado es un Symbol, lo usa directamente como clave (los Symbols son un tipo de clave distinto a string).
4. Asigna la propiedad con esa clave y el valor `val`.

El proceso ocurre en orden de aparición de las propiedades en el literal, de izquierda a derecha. Si dos expresiones producen la misma clave, la última sobreescribe a la anterior.

> [!tip] Buenas prácticas
> - Usar dispatch tables con propiedades calculadas en lugar de `switch` para código más extensible.
> - Combinar con `Object.fromEntries` para transformaciones de colecciones sin mutación.
> - Usar Symbols calculados para implementar protocolos del lenguaje (`Symbol.iterator`, `Symbol.toPrimitive`) directamente en literales.

> [!warning] Errores comunes
> - Usar un objeto como clave calculada: `{ [{}]: val }` — el objeto se convierte a `"[object Object]"`, lo que causa colisiones inesperadas.
> - Asumir que claves numéricas se mantienen como números: `{ [1]: "a" }` — la clave se almacena como string `"1"`.
> - En claves duplicadas calculadas, no hay error: la última escritura silenciosamente sobreescribe a las anteriores.

## Notas relacionadas

- [[03 Acceso (dot y bracket)]] — acceder a propiedades con claves dinámicas en bracket
- [[05 Shorthand de Propiedades y Métodos]] — azúcar sintáctico complementario al crear literales
- [[01 Programación Procedimental/03 Tipos de Datos/01 Primitivos/06 symbol.md | symbol]] — Symbols como claves especiales
