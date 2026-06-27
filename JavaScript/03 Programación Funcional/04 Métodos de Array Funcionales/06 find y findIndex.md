---
title: "find y findIndex"
aliases:
  - Array find
  - Array findIndex
  - array.find
  - array.findIndex
tags:
  - javascript
  - api/metodo
  - arrays
draft: false
order: 6
---

# find y findIndex

> [!definicion]
> `Array.prototype.find` devuelve el **primer elemento** del array para el que el callback retorna truthy. `Array.prototype.findIndex` devuelve el **índice** de ese primer elemento. Ambos hacen cortocircuito — detienen la iteración en cuanto encuentran una coincidencia. Si no encuentran nada, `find` devuelve `undefined` y `findIndex` devuelve `-1`.

```js
const usuarios = [
  { id: 1, nombre: "Ana", activo: false },
  { id: 2, nombre: "Luis", activo: true },
  { id: 3, nombre: "Eva", activo: true },
];

const primerActivo = usuarios.find((u) => u.activo);
// {id: 2, nombre: "Luis", activo: true}

const índicePrimerActivo = usuarios.findIndex((u) => u.activo);
// 1

// Sin coincidencia:
usuarios.find((u) => u.id === 99);       // undefined
usuarios.findIndex((u) => u.id === 99);  // -1
```

## Firma del método

| Parámetro del callback | Tipo | Descripción |
|---|---|---|
| `elemento` | any | Valor del elemento actual |
| `índice` | number | Posición (0-based) |
| `array` | Array | Referencia al array original |

| Aspecto | `find` | `findIndex` |
|---|---|---|
| Devuelve si encuentra | El elemento (cualquier tipo) | El índice (`number`) |
| Devuelve si no encuentra | `undefined` | `-1` |
| Cortocircuito | Sí | Sí |
| Muta el array | No | No |
| Salta huecos (array sparse) | Sí | Sí |

## Ejemplos

### Buscar objeto por propiedad

```js
const productos = [
  { sku: "A100", nombre: "Laptop", stock: 5 },
  { sku: "B200", nombre: "Mouse", stock: 0 },
  { sku: "C300", nombre: "Teclado", stock: 12 },
];

const laptop = productos.find((p) => p.sku === "A100");
// {sku: "A100", nombre: "Laptop", stock: 5}

const índiceSinStock = productos.findIndex((p) => p.stock === 0);
// 1
```

### Buscar por condición compuesta

```js
const empleados = [
  { nombre: "Ana", departamento: "IT", nivel: 3 },
  { nombre: "Luis", departamento: "IT", nivel: 5 },
  { nombre: "Eva", departamento: "HR", nivel: 4 },
];

const itSenior = empleados.find(
  (e) => e.departamento === "IT" && e.nivel >= 5
);
// {nombre: "Luis", departamento: "IT", nivel: 5}
```

### Usar `findIndex` para actualizar un elemento por posición

```js
const carrito = [
  { id: 1, producto: "Laptop", cantidad: 1 },
  { id: 2, producto: "Mouse", cantidad: 2 },
];

const idx = carrito.findIndex((item) => item.id === 2);
if (idx !== -1) {
  carrito[idx] = { ...carrito[idx], cantidad: 3 };
}
// carrito[1] = {id: 2, producto: "Mouse", cantidad: 3}
```

### Buscar el primer número que cumple una condición matemática

```js
const datos = [0.1, 0.3, 0.7, 0.9, 0.5];

const primerSuperiorAMitad = datos.find((n) => n > 0.5);
// 0.7

const índice = datos.findIndex((n) => n > 0.5);
// 2
```

## Cortocircuito y eficiencia

`find` y `findIndex` son más eficientes que `filter()[0]` cuando solo necesitas el primer elemento, porque detienen la iteración en cuanto encuentran la primera coincidencia:

```js
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// filter recorre TODOS los elementos, crea un array, luego accede al [0]
arr.filter((n) => n > 5)[0];
// Visita: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 → resultado: [6,7,8,9,10] → toma 6

// find para en el primero que cumple
arr.find((n) => n > 5);
// Visita: 1, 2, 3, 4, 5, 6 → para → devuelve 6
```

En arrays grandes o con callbacks costosos, esta diferencia puede ser significativa.

## `find` vs `indexOf`

```js
const frutas = ["manzana", "banana", "cereza"];

// indexOf: igualdad estricta (===), solo para valores primitivos
frutas.indexOf("banana");         // 1
frutas.indexOf("uva");            // -1

// find: predicado arbitrario, funciona con cualquier condición
frutas.find((f) => f.startsWith("b"));  // "banana"

// Para objetos, indexOf es casi inútil (compara referencias)
const users = [{id: 1}, {id: 2}];
users.indexOf({id: 1});           // -1 (referencias distintas)
users.findIndex((u) => u.id === 1);  // 0 ✓
```

`find` es el reemplazo moderno de `indexOf` para cualquier búsqueda con lógica personalizada. `indexOf` sigue siendo útil para búsquedas de igualdad simple en primitivos.

## Cómo funciona por dentro

Ambos métodos iteran desde el índice `0` hasta `length - 1`. En cada índice que sea propiedad propia del array, invocan `callback(array[i], i, array)`. En cuanto el callback devuelve un valor truthy, `find` devuelve `array[i]` y `findIndex` devuelve `i`, deteniendo la iteración. Si se llega al final sin encontrar, `find` devuelve `undefined` y `findIndex` devuelve `-1`.

```js
// Implementación conceptual de find
Array.prototype.findPropio = function(callback) {
  for (let i = 0; i < this.length; i++) {
    if (i in this && callback(this[i], i, this)) {
      return this[i];   // cortocircuito aquí
    }
  }
  return undefined;
};
```

Un detalle importante: `find` puede devolver `undefined` tanto si no encuentra nada como si el elemento que cumple la condición es `undefined`. Para distinguir ambos casos, usar `findIndex` y comprobar si el índice es `-1`:

```js
const arr = [undefined, 1, 2];
arr.find((x) => x === undefined);       // undefined (encontró)
arr.findIndex((x) => x === undefined);  // 0 (encontró en posición 0, no es -1)
```

> [!tip] Buenas prácticas
> - Usar `find` cuando solo necesitas el valor del primer elemento que cumple la condición.
> - Usar `findIndex` cuando necesitas la posición para modificar o eliminar el elemento del array original.
> - Preferir `find`/`findIndex` sobre `filter()[0]`/`filter` + búsqueda de índice por su cortocircuito.
> - Cuando el resultado puede ser `undefined` de forma legítima, usar `findIndex !== -1` para saber si se encontró algo.
> - Para buscar la última ocurrencia, usar `findLast`/`findLastIndex` (ES2023) en vez de invertir el array.

> [!warning] Errores comunes
> - **Asumir que `find` devuelve `null` cuando no encuentra:** devuelve `undefined`. Comprobar con `=== undefined` o el operador `??`.
> - **Usar `filter()[0]` cuando basta con `find`:** es menos eficiente y menos expresivo.
> - **Ambigüedad cuando el valor buscado es `undefined`:** usar `findIndex` para determinar si la búsqueda tuvo éxito.
> - **Mutar el objeto devuelto por `find` sin querer:** `find` devuelve el mismo objeto del array (referencia), no una copia. Modificarlo muta el original.
> - **`find` sobre strings:** para buscar un carácter o subcadena en un string, usar `String.includes`, `String.indexOf` o regex — `find` es solo para arrays.

## Notas relacionadas

- [[index|Métodos de Array Funcionales — Índice]]
- [[03 filter]]
- [[07 findLast y findLastIndex]]
- [[08 some]]
- [[09 every]]
