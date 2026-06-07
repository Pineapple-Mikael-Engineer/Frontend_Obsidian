---
title: toSorted, toReversed, toSpliced, with — Versiones inmutables (ES2023)
aliases:
  - toSorted
  - toReversed
  - toSpliced
  - Array.with
  - métodos inmutables ES2023
tags:
  - javascript
  - api/metodo
  - funcional
draft: false
---

# toSorted, toReversed, toSpliced y with

> [!definicion]
> ES2023 introdujo cuatro métodos que son versiones **inmutables** de sus equivalentes mutantes: `toSorted`, `toReversed`, `toSpliced` y `with`. Cada uno devuelve un **nuevo array** con el cambio aplicado y deja el original intacto. Resuelven el problema histórico de que `sort`, `reverse` y `splice` mutan in-place.

```js
const nums = [3, 1, 4, 1, 5];

const ordenado = nums.toSorted();   // [1, 1, 3, 4, 5]
const invertido = nums.toReversed(); // [5, 1, 4, 1, 3]

console.log(nums); // [3, 1, 4, 1, 5] — intacto en ambos casos
```

## Tabla comparativa: mutante vs inmutable

| Método mutante | Efecto del original | Versión inmutable | ES |
|---|---|---|---|
| `sort(compareFn?)` | Lo ordena in-place | `toSorted(compareFn?)` | 2023 |
| `reverse()` | Lo invierte in-place | `toReversed()` | 2023 |
| `splice(ini, del, ...ins)` | Elimina/inserta in-place | `toSpliced(ini, del, ...ins)` | 2023 |
| (no existe) | — | `with(índice, valor)` | 2023 |

---

## toSorted

`array.toSorted(compareFn?)` — ordena igual que `sort` pero devuelve un nuevo array.

```js
const palabras = ["banana", "apple", "cherry"];
const alfa = palabras.toSorted();
// ["apple", "banana", "cherry"]

const nums = [10, 2, 30, 4];
const asc = nums.toSorted((a, b) => a - b);
// [2, 4, 10, 30]
const desc = nums.toSorted((a, b) => b - a);
// [30, 10, 4, 2]

console.log(nums); // [10, 2, 30, 4] — no mutado
```

Sin `compareFn`, convierte los elementos a string y los ordena lexicográficamente (igual que `sort`).

## toReversed

`array.toReversed()` — invierte igual que `reverse` pero devuelve un nuevo array.

```js
const letras = ["a", "b", "c", "d"];
const invertidas = letras.toReversed();
// ["d", "c", "b", "a"]

console.log(letras); // ["a", "b", "c", "d"] — no mutado
```

## toSpliced

`array.toSpliced(inicio, eliminar, ...insertar)` — misma firma que `splice` pero devuelve el nuevo array con los cambios. A diferencia de `splice`, no devuelve los elementos eliminados — devuelve el array resultante completo.

```js
const colores = ["rojo", "verde", "azul", "amarillo"];

// Eliminar 1 elemento en índice 1
colores.toSpliced(1, 1);
// ["rojo", "azul", "amarillo"]

// Reemplazar 1 elemento en índice 2
colores.toSpliced(2, 1, "morado");
// ["rojo", "verde", "morado", "amarillo"]

// Insertar sin eliminar
colores.toSpliced(2, 0, "naranja", "rosa");
// ["rojo", "verde", "naranja", "rosa", "azul", "amarillo"]

console.log(colores); // ["rojo", "verde", "azul", "amarillo"] — no mutado
```

## with

`array.with(índice, valor)` — devuelve un nuevo array con el elemento en `índice` reemplazado por `valor`. Acepta índices negativos.

```js
const arr = [10, 20, 30, 40];

arr.with(1, 99);   // [10, 99, 30, 40]
arr.with(-1, 99);  // [10, 20, 30, 99] — índice negativo desde el final

console.log(arr);  // [10, 20, 30, 40] — no mutado
```

Es equivalente a `[...arr.slice(0, i), valor, ...arr.slice(i + 1)]` pero más conciso y legible.

## Recetas

### Pipeline de transformación sin efectos

```js
const datos = [5, 3, 8, 1, 9, 2];

const resultado = datos
  .toSorted((a, b) => a - b)   // orden ascendente, sin mutar datos
  .toSpliced(0, 2)              // eliminar los 2 menores
  .toReversed();                // orden descendente del resto

// datos intacto en todo momento
```

### Estado inmutable en Redux / gestores de estado

```js
// reducer de lista de tareas
function reducer(estado, accion) {
  switch (accion.type) {
    case "REORDENAR":
      return { ...estado, tareas: estado.tareas.toSorted(accion.comparador) };
    case "ACTUALIZAR_ITEM":
      return { ...estado, items: estado.items.with(accion.indice, accion.valor) };
    default:
      return estado;
  }
}
```

### Corregir el problema clásico de `sort` en React

```js
// MAL: sort muta el array de estado directamente
setItems(items.sort((a, b) => a.precio - b.precio));

// BIEN: toSorted devuelve un nuevo array
setItems(items.toSorted((a, b) => a.precio - b.precio));
```

## Compatibilidad

| Entorno | Soporte |
|---|---|
| Chrome | 110+ |
| Firefox | 115+ |
| Safari | 16+ |
| Node.js | 20+ |
| Edge | 110+ |

Para entornos más antiguos, polyfill mínimo de `toSorted`:

```js
if (!Array.prototype.toSorted) {
  Array.prototype.toSorted = function(compareFn) {
    return [...this].sort(compareFn);
  };
}
```

El mismo patrón aplica para los demás: copiar con spread y aplicar el mutante sobre la copia.

## Cómo funcionan por dentro

Internamente, cada método crea una copia del array original (equivalente a `[...arr]`) y luego aplica la operación mutante sobre esa copia. El coste total es O(n) de copia más el coste propio del algoritmo (O(n log n) para `toSorted`, O(n) para `toReversed`, O(n) para `toSpliced` y `with`).

> [!tip] Buenas prácticas
> - En cualquier contexto de estado inmutable (React, Redux, Zustand), preferir `toSorted`, `toReversed`, `toSpliced` y `with` sobre sus equivalentes mutantes.
> - `with` es especialmente útil para actualizar un elemento puntual de un array sin necesidad de destructurar con spread y slice.
> - Encadenarlos es seguro: cada llamada produce un nuevo array, por lo que no hay efectos secundarios entre pasos.

> [!warning] Errores comunes
> - **Confundir el retorno de `toSpliced` con `splice`:** `splice` devuelve los elementos eliminados; `toSpliced` devuelve el array resultante completo. Son valores completamente distintos.
> - **`sort` sigue mutando:** el hecho de que exista `toSorted` no cambia el comportamiento de `sort`. Si se usa el original, sigue mutando.
> - **`with` con índice fuera de rango lanza `RangeError`:** a diferencia de la asignación directa (`arr[100] = x`) que crea huecos, `with` es estricto con los límites.
> - **Disponibilidad:** en proyectos que soportan Node 18 o navegadores anteriores a 2023, verificar compatibilidad o añadir polyfill antes de usar.

## Notas relacionadas

- [[index|Inmutabilidad Práctica — Índice]]
- [[01 Spread Operator]]
- [[04 concat y slice]]
- [[../04 Métodos de Array Funcionales/index|Métodos de Array Funcionales — Índice]]
