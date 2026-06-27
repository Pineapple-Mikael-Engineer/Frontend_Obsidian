---
title: Array.prototype.sort — Ordenar in-place
aliases:
  - sort
  - Array.sort
  - toSorted
tags:
  - javascript
  - api/metodo
  - arrays
objeto: Array
tipo: metodo
retorna: Array
muta: true
asincrono: false
draft: false
order: 10
---

# Array.prototype.sort

> [!definicion]
> `arr.sort(compareFn?)` ordena el array **in-place** y retorna el mismo array (no una copia). Sin `compareFn`, convierte los elementos a string y los ordena lexicográficamente — trampa clásica con números. Con `compareFn(a, b)`: si retorna negativo, `a` precede a `b`; si positivo, `b` precede a `a`; si `0`, el orden relativo se preserva (sort estable desde ES2019). **Muta** el array.

## Firma

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `compareFn` | `(a, b) => number` (opcional) | Función de comparación. Omitida: orden lexicográfico |

**Retorna:** el mismo array mutado. **Muta.**

## Sin `compareFn` — orden lexicográfico

```js
["banana", "manzana", "cereza"].sort();
// → ["banana", "cereza", "manzana"]   (correcto para strings)

[10, 9, 2, 100, 21].sort();
// → [10, 100, 2, 21, 9]   ← INCORRECTO para números
// Los números se convierten a string: "10" < "100" < "2" < "21" < "9"
```

## Con `compareFn` — orden numérico

La `compareFn` recibe dos elementos `a` y `b`. Su contrato:

- Retorna negativo → `a` va antes que `b`
- Retorna positivo → `b` va antes que `a`
- Retorna `0` → orden relativo preservado

```js
const nums = [10, 9, 2, 100, 21];

// Ascendente
nums.sort((a, b) => a - b);
// → [2, 9, 10, 21, 100]

// Descendente
nums.sort((a, b) => b - a);
// → [100, 21, 10, 9, 2]
```

## Ordenar objetos por propiedad

```js
const personas = [
  { nombre: "Carlos", edad: 30 },
  { nombre: "Ana",    edad: 25 },
  { nombre: "Beatriz", edad: 28 },
];

// Por nombre (alfabético, respetando locale)
personas.sort((a, b) => a.nombre.localeCompare(b.nombre));
// → [Ana, Beatriz, Carlos]

// Por edad ascendente
personas.sort((a, b) => a.edad - b.edad);
// → [Ana(25), Beatriz(28), Carlos(30)]
```

## Ordenar con múltiples criterios

```js
// Primero por categoría, luego por precio ascendente
productos.sort((a, b) => {
  if (a.categoria !== b.categoria) return a.categoria.localeCompare(b.categoria);
  return a.precio - b.precio;
});
```

## Estabilidad del sort (ES2019+)

Desde ES2019, la especificación garantiza que `sort` sea **estable**: elementos con `compareFn` = 0 mantienen su orden relativo original. V8 usa **TimSort** (merge sort + insertion sort adaptativo, O(n log n)).

```js
const items = [
  { id: 1, grupo: "A" },
  { id: 2, grupo: "B" },
  { id: 3, grupo: "A" },
];
items.sort((a, b) => a.grupo.localeCompare(b.grupo));
// → [{id:1, grupo:"A"}, {id:3, grupo:"A"}, {id:2, grupo:"B"}]
// Los dos del grupo "A" mantienen su orden (1 antes que 3)
```

## Alternativa inmutable: `toSorted` (ES2023)

`arr.toSorted(compareFn)` retorna un **nuevo array** ordenado sin mutar el original:

```js
const original = [3, 1, 2];
const ordenado = original.toSorted((a, b) => a - b);

console.log(ordenado);  // → [1, 2, 3]
console.log(original);  // → [3, 1, 2]  (sin cambios)
```

## Recetas comunes

```js
// Aleatorizar (Fisher-Yates no puro — suficiente para UI, no criptografía)
arr.sort(() => Math.random() - 0.5);

// Ordenar strings ignorando mayúsculas
arr.sort((a, b) => a.toLowerCase().localeCompare(b.toLowerCase()));

// Ordenar por longitud de string
arr.sort((a, b) => a.length - b.length);

// Copia ordenada sin mutar (pre-ES2023)
const copia = [...arr].sort((a, b) => a - b);
```

> [!tip] Buenas prácticas
> - Siempre proporcionar `compareFn` cuando se ordenen números o por criterio no-lexicográfico.
> - Usar `localeCompare` para strings con acentos o caracteres no ASCII.
> - Para no mutar: `[...arr].sort(fn)` (pre-ES2023) o `arr.toSorted(fn)` (ES2023+).
> - El sort de `Math.random() - 0.5` no está uniformemente distribuido — para aleatoriedad correcta usar Fisher-Yates explícito.

> [!warning] Errores comunes
> - `[10, 9, 2].sort()` — sin `compareFn`, el resultado es lexicográfico, no numérico. Nunca omitir la función al ordenar números.
> - `sort` retorna el mismo array: `const copia = arr.sort(fn)` hace que `copia` y `arr` sean el mismo objeto. Para copia, usar `[...arr].sort(fn)`.
> - Una `compareFn` que no respeta transitividad puede dar resultados indeterminados o bucles infinitos en algunos motores.
> - Mutar el array mientras `sort` está en ejecución (dentro de la propia `compareFn`) produce comportamiento indefinido.

## Notas relacionadas

- [[11 reverse]]
- [[08 includes, indexOf, lastIndexOf]]
- [[index|Arrays]]
