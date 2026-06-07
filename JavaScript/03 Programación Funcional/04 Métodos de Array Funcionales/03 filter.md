---
title: "filter"
aliases:
  - Array filter
  - array.filter
tags:
  - javascript
  - api/metodo
  - arrays
draft: false
---

# filter

> [!definicion]
> `Array.prototype.filter` devuelve un **nuevo array** que contiene únicamente los elementos del original para los cuales el callback devuelve un valor truthy. Actúa como un **predicado** aplicado a cada elemento — si pasa el test, se incluye; si no, se descarta. El array original no se modifica.

```js
const numeros = [1, 2, 3, 4, 5, 6, 7, 8];
const pares = numeros.filter((n) => n % 2 === 0);
// [2, 4, 6, 8]

console.log(numeros); // [1, 2, 3, 4, 5, 6, 7, 8] — sin cambios
```

## Firma del método

| Parámetro del callback | Tipo | Descripción |
|---|---|---|
| `elemento` | any | Valor del elemento actual |
| `índice` | number | Posición (0-based) |
| `array` | Array | Referencia al array original |

| Aspecto | Detalle |
|---|---|
| Valor de retorno del método | Nuevo `Array` con los elementos que pasan el predicado |
| Tamaño del resultado | Entre 0 y `length` del original |
| Ninguno pasa el predicado | Devuelve array vacío `[]`, nunca `null`/`undefined` |
| Muta el array | No |
| Argumento `thisArg` | Opcional |
| Salta huecos (array sparse) | Sí |

## Ejemplos

### Filtrar objetos por propiedad

```js
const usuarios = [
  { nombre: "Ana", activo: true },
  { nombre: "Luis", activo: false },
  { nombre: "Eva", activo: true },
  { nombre: "Tomás", activo: false },
];

const activos = usuarios.filter((u) => u.activo);
// [{nombre: "Ana", activo: true}, {nombre: "Eva", activo: true}]
```

### Filtrar por tipo

```js
const mixto = [1, "hola", null, true, undefined, 42, "", false];

const soloNumeros = mixto.filter((x) => typeof x === "number");
// [1, 42]

const soloStringsNoVacías = mixto.filter((x) => typeof x === "string" && x.length > 0);
// ["hola"]
```

### Eliminar valores falsy con `Boolean`

```js
const conFalsy = [0, 1, "", "texto", null, undefined, false, NaN, 42];
const soloTruthy = conFalsy.filter(Boolean);
// [1, "texto", 42]
```

`Boolean` como callback es el equivalente funcional de `(x) => !!x` o `(x) => Boolean(x)`. A diferencia de `parseInt`, `Boolean` solo usa el primer argumento, así que pasarlo directamente es seguro.

### Filtrar por índice (cada N elementos)

```js
const datos = [10, 20, 30, 40, 50, 60];
const cadaTercero = datos.filter((_, i) => i % 3 === 0);
// [10, 40]
```

## Recetas comunes

### Eliminar duplicados (combinado con `indexOf`)

```js
const conRepetidos = [1, 2, 2, 3, 1, 4, 3];
const únicos = conRepetidos.filter((v, i, arr) => arr.indexOf(v) === i);
// [1, 2, 3, 4]
// La lógica: solo se mantiene el elemento cuya primera aparición está en la posición actual
```

Para datos complejos con muchos duplicados, `Set` o `Map` son más eficientes — pero este patrón con `filter` es idiomático para arrays pequeños.

### Filtrar objetos por múltiples condiciones

```js
const productos = [
  { nombre: "Laptop", precio: 1200, disponible: true },
  { nombre: "Mouse", precio: 25, disponible: false },
  { nombre: "Teclado", precio: 80, disponible: true },
  { nombre: "Monitor", precio: 350, disponible: true },
];

const oferta = productos.filter(
  (p) => p.disponible && p.precio >= 50 && p.precio <= 500
);
// [{nombre: "Teclado", ...}, {nombre: "Monitor", ...}]
```

### Filtrar elementos que están en otra lista (diferencia de conjuntos)

```js
const todos = [1, 2, 3, 4, 5, 6];
const excluidos = new Set([2, 4, 6]);
const restantes = todos.filter((n) => !excluidos.has(n));
// [1, 3, 5]
```

### Búsqueda de texto en array de objetos

```js
const buscar = (lista, término) =>
  lista.filter((item) =>
    item.nombre.toLowerCase().includes(término.toLowerCase())
  );

buscar(productos, "key"); // [{nombre: "Teclado", ...}]
```

### Filtrar propiedades definidas de un objeto (sin usar filter directamente)

```js
// En objetos no existe filter, pero el patrón equivalente es:
const config = { host: "localhost", port: 3000, debug: undefined, timeout: null };
const limpio = Object.fromEntries(
  Object.entries(config).filter(([, v]) => v != null)
);
// {host: "localhost", port: 3000}
```

## Diferencia con `find`

`filter` y `find` son conceptualmente similares pero tienen propósitos distintos:

```js
const nums = [1, 3, 5, 4, 2];

// filter: todos los que cumplen → devuelve array
const mayoresDeTres = nums.filter((n) => n > 3);
// [5, 4]

// find: solo el primero que cumple → devuelve el elemento
const primerMayorDeTres = nums.find((n) => n > 3);
// 5

// Si no hay coincidencia:
[1, 2].filter((n) => n > 10);  // [] — array vacío
[1, 2].find((n) => n > 10);    // undefined
```

Cuando solo necesitas saber si existe al menos uno, `some` es más semántico y eficiente que `filter(...).length > 0`.

## Cómo funciona por dentro

`filter` crea un nuevo array vacío. Itera desde el índice `0` hasta `length - 1`. Para cada índice que sea propiedad propia del array, invoca el callback con `(array[i], i, array)`. Si el valor de retorno del callback es truthy, el elemento se añade al nuevo array (manteniendo el orden original). Los huecos se saltan. Al finalizar, devuelve el nuevo array (que puede estar vacío).

```js
// Implementación conceptual de filter
Array.prototype.filterPropio = function(callback, thisArg) {
  const resultado = [];
  for (let i = 0; i < this.length; i++) {
    if (i in this && callback.call(thisArg, this[i], i, this)) {
      resultado.push(this[i]);
    }
  }
  return resultado;
};
```

El resultado nunca contiene huecos aunque el original los tuviera — `filter` produce siempre un array denso (sin huecos).

> [!tip] Buenas prácticas
> - Usar `filter(Boolean)` para limpiar arrays de valores falsy de forma concisa y legible.
> - Colocar `filter` antes de `map` en un pipeline: filtrar primero reduce el número de elementos que `map` tiene que procesar.
> - Para comprobar existencia sin necesitar el valor, usar `some` en lugar de `filter(...).length > 0` — `some` hace cortocircuito.
> - Para eliminar duplicados de objetos complejos, usar `Map` o `Set` en lugar del patrón `filter` + `indexOf` por eficiencia.
> - El callback debe ser un predicado puro (sin efectos secundarios): predecible, testeable, sin estado externo.

> [!warning] Errores comunes
> - **Esperar `null` cuando nada pasa:** `filter` devuelve un array vacío `[]` cuando ningún elemento pasa — comprobar `.length` para saber si el resultado tiene elementos.
> - **Usar `filter` para obtener un solo elemento:** si solo necesitas el primero, `find` es más eficiente (cortocircuito) y más explícito en la intención.
> - **Confundir valores falsy del dominio con "no pasa el filtro":** si los datos pueden ser `0`, `false` o `""` valores legítimos, el callback debe ser explícito: `(x) => x !== null` en vez de `(x) => x`.
> - **Olvidar que el resultado es una copia:** modificar objetos dentro del resultado puede mutar los objetos del original si son referencias (shallow copy).
> - **`filter` no existe en NodeList/HTMLCollection:** para filtros sobre elementos del DOM, convertir primero: `Array.from(document.querySelectorAll(...)).filter(...)`.

## Notas relacionadas

- [[index|Métodos de Array Funcionales — Índice]]
- [[02 map]]
- [[04 reduce]]
- [[06 find y findIndex]]
- [[08 some]]
- [[09 every]]
- [[12 Encadenamiento de Métodos]]
