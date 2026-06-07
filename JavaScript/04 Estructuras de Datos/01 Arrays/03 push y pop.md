---
title: Array.prototype.push y pop — Añadir y eliminar al final
aliases:
  - push
  - pop
  - Array.push
  - Array.pop
tags:
  - javascript
  - api/metodo
  - arrays
objeto: Array
tipo: metodo
retorna: number | any
muta: true
asincrono: false
draft: false
---

# Array.prototype.push y pop

> [!definicion]
> `push(...elementos)` añade uno o más elementos al **final** del array y retorna la nueva `length`. `pop()` elimina el último elemento, lo retorna y decrementa `length`. Ambos **mutan** el array original. Operación O(1) amortizado.

## Firmas

| Método | Firma | Retorna | Muta |
|--------|-------|---------|------|
| `push` | `arr.push(...items)` | `number` — nueva `length` | **sí** |
| `pop` | `arr.pop()` | elemento eliminado o `undefined` | **sí** |

## push — añadir al final

```js
const pila = [1, 2, 3];

const nuevaLong = pila.push(4);
console.log(pila);      // → [1, 2, 3, 4]
console.log(nuevaLong); // → 4

// push acepta múltiples argumentos
pila.push(5, 6, 7);
console.log(pila); // → [1, 2, 3, 4, 5, 6, 7]
```

El valor de retorno de `push` es la nueva longitud, no el array. Error habitual: encadenar con `.filter` o similar porque `push` no retorna el array.

## pop — eliminar el último

```js
const pila = ["a", "b", "c"];

const ultimo = pila.pop();
console.log(ultimo); // → "c"
console.log(pila);   // → ["a", "b"]

// pop en array vacío
[].pop(); // → undefined  (sin error)
```

## Receta: pila LIFO

`push` + `pop` implementan una pila Last-In First-Out de forma directa:

```js
const historial = [];

historial.push("pagina1");
historial.push("pagina2");
historial.push("pagina3");

// Navegar hacia atrás
const actual = historial.pop(); // → "pagina3"
const prev   = historial.pop(); // → "pagina2"
console.log(historial);         // → ["pagina1"]
```

## Receta: acumular resultados en un bucle

```js
const pares = [];
for (let i = 0; i <= 10; i++) {
  if (i % 2 === 0) pares.push(i);
}
console.log(pares); // → [0, 2, 4, 6, 8, 10]
```

Para transformaciones declarativas, `filter` y `map` son más idiomáticos. `push` en bucle es apropiado cuando la lógica de acumulación es compleja o condicional con estado.

## Alternativas inmutables

```js
const original = [1, 2, 3];

// Equivalente inmutable a push(4)
const conCuatro = [...original, 4];      // → [1, 2, 3, 4]

// Equivalente inmutable a pop()
const sinUltimo = original.slice(0, -1); // → [1, 2]

// original no se modifica en ningún caso
console.log(original); // → [1, 2, 3]
```

> [!tip] Buenas prácticas
> - Usar `push` cuando el rendimiento importa y la mutación es aceptable (bucles de acumulación).
> - Usar spread o `concat` cuando se trabaja con estado inmutable (React, Redux).
> - El retorno de `push` (la nueva longitud) raramente se usa; si se necesita el array, es el mismo objeto mutado.

> [!warning] Errores comunes
> - `arr.push([1, 2])` añade el array como un **único** elemento anidado, no aplana. Para concatenar elementos de otro array usar `arr.push(...otroArr)` o `arr.concat(otroArr)`.
> - `const resultado = arr.push(x)` — `resultado` es un `number`, no el array. El error aparece al intentar encadenar métodos sobre él.
> - `pop` en un array vacío retorna `undefined` sin error; conviene verificar `arr.length > 0` antes en código que depende del valor.

## Cómo funciona por dentro

`push(x)` escribe `arr[arr.length] = x` y luego actualiza `length`. `pop()` lee `arr[arr.length - 1]`, elimina esa propiedad con `delete arr[arr.length - 1]`, decrementa `length` y retorna el valor leído. Ambas operaciones son O(1) amortizado porque operan sobre el extremo final sin desplazar otros elementos.

## Notas relacionadas

- [[04 shift y unshift]]
- [[05 splice]]
- [[06 slice]]
- [[index|Arrays]]
