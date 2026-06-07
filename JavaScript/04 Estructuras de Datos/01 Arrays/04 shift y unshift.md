---
title: Array.prototype.shift y unshift — Eliminar y añadir al inicio
aliases:
  - shift
  - unshift
  - Array.shift
  - Array.unshift
tags:
  - javascript
  - api/metodo
  - arrays
objeto: Array
tipo: metodo
retorna: any | number
muta: true
asincrono: false
draft: false
---

# Array.prototype.shift y unshift

> [!definicion]
> `shift()` elimina el **primer** elemento del array, lo retorna y desplaza todos los demás un índice hacia abajo. `unshift(...elementos)` inserta uno o más elementos al **inicio**, desplazando los existentes hacia arriba, y retorna la nueva `length`. Ambos **mutan** el array. Operación O(n) — el desplazamiento es lineal en el número de elementos.

## Firmas

| Método | Firma | Retorna | Muta |
|--------|-------|---------|------|
| `shift` | `arr.shift()` | elemento eliminado o `undefined` | **sí** |
| `unshift` | `arr.unshift(...items)` | `number` — nueva `length` | **sí** |

## shift — eliminar el primero

```js
const cola = ["primero", "segundo", "tercero"];

const extraido = cola.shift();
console.log(extraido); // → "primero"
console.log(cola);     // → ["segundo", "tercero"]

// shift en array vacío
[].shift(); // → undefined  (sin error)
```

Tras `shift`, el motor renombra internamente todas las propiedades numéricas: la propiedad `"1"` pasa a ser `"0"`, `"2"` a `"1"`, etc. Por eso el costo es O(n).

## unshift — añadir al inicio

```js
const lista = [3, 4, 5];

const nuevaLong = lista.unshift(1, 2);
console.log(lista);     // → [1, 2, 3, 4, 5]
console.log(nuevaLong); // → 5

// Con un solo elemento
lista.unshift(0);
console.log(lista); // → [0, 1, 2, 3, 4, 5]
```

Los argumentos se insertan en orden: `arr.unshift(a, b)` coloca primero `a`, luego `b`.

## Receta: cola FIFO

`push` + `shift` implementan una cola First-In First-Out:

```js
const cola = [];

cola.push("tarea1");
cola.push("tarea2");
cola.push("tarea3");

while (cola.length > 0) {
  const tarea = cola.shift();
  console.log(`Procesando: ${tarea}`);
}
// → Procesando: tarea1
// → Procesando: tarea2
// → Procesando: tarea3
```

> [!warning] Rendimiento en colas grandes
> Para colas de alto rendimiento con muchas operaciones `shift`, este patrón tiene costo O(n) por operación. Con decenas de miles de elementos, considerar una implementación de deque con un puntero de cabeza o una estructura `LinkedList`.

## Alternativas inmutables

```js
const original = [1, 2, 3];

// Equivalente inmutable a shift()
const [primero, ...resto] = original;
// primero → 1, resto → [2, 3], original sin cambios

// Equivalente inmutable a shift() con slice
const sinPrimero = original.slice(1); // → [2, 3]

// Equivalente inmutable a unshift(0)
const conCero = [0, ...original]; // → [0, 1, 2, 3]
```

> [!tip] Buenas prácticas
> - Usar `shift`/`unshift` solo cuando la mutación es intencional y el array es pequeño.
> - Para procesamiento de colas sobre arrays grandes (>10.000 elementos con operaciones frecuentes), preferir un índice de cabeza manual o una librería de deque.
> - Si se trabaja con estado inmutable, destructuring o spread son más claros.

> [!warning] Errores comunes
> - Usar `shift` en un bucle `while (arr.length)` sobre un array grande → O(n²) total. Es el antipatrón clásico para colas ineficientes.
> - `unshift` con múltiples argumentos los inserta en orden, no invertido: `[3].unshift(1, 2)` → `[1, 2, 3]`, no `[2, 1, 3]`.
> - Confundir el valor de retorno: `shift` retorna el elemento eliminado; `unshift` retorna la nueva longitud (igual que `push`).

## Cómo funciona por dentro

`shift` lee el elemento en el índice 0, luego re-asigna todas las propiedades numéricas una posición hacia abajo (`arr["0"] = arr["1"]`, ..., elimina el último) y decrementa `length`. `unshift` hace el proceso inverso: desplaza hacia arriba y escribe en los primeros índices. Ambos recorren el array completo: O(n).

## Notas relacionadas

- [[03 push y pop]]
- [[05 splice]]
- [[06 slice]]
- [[index|Arrays]]
