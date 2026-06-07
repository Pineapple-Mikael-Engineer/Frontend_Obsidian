---
title: Array.prototype.splice — Eliminar, insertar y reemplazar en posición arbitraria
aliases:
  - splice
  - Array.splice
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
---

# Array.prototype.splice

> [!definicion]
> `arr.splice(inicio, eliminar?, ...insertar)` elimina `eliminar` elementos a partir del índice `inicio`, opcionalmente inserta los elementos `insertar` en esa posición, y retorna un array con los elementos eliminados. **Muta** el array original. Es la navaja suiza de las operaciones de posición arbitraria.

## Firma

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `inicio` | `number` | Índice de inicio. Negativo: cuenta desde el final |
| `eliminar` | `number` (opcional) | Cantidad de elementos a eliminar. Omitido: elimina hasta el final |
| `...insertar` | `any` (opcional) | Elementos a insertar en la posición `inicio` |

**Retorna:** array con los elementos eliminados (vacío si no se eliminó nada). **Muta** el array.

## Casos de uso

### Eliminar por índice

```js
const arr = ["a", "b", "c", "d", "e"];

// Eliminar 1 elemento en el índice 2
const eliminados = arr.splice(2, 1);
console.log(eliminados); // → ["c"]
console.log(arr);        // → ["a", "b", "d", "e"]
```

### Eliminar múltiples elementos

```js
const arr = [1, 2, 3, 4, 5];

arr.splice(1, 3); // desde índice 1, eliminar 3
console.log(arr); // → [1, 5]
```

### Insertar sin eliminar

Con `eliminar = 0`, solo se insertan elementos sin eliminar ninguno:

```js
const arr = ["a", "c", "d"];

arr.splice(1, 0, "b");
console.log(arr); // → ["a", "b", "c", "d"]
```

### Reemplazar elementos

```js
const arr = ["rojo", "verde", "azul"];

arr.splice(1, 1, "amarillo", "naranja");
console.log(arr); // → ["rojo", "amarillo", "naranja", "azul"]
```

### Índice negativo

Un `inicio` negativo cuenta desde el final del array:

```js
const arr = [1, 2, 3, 4, 5];

arr.splice(-2, 1); // eliminar el penúltimo
console.log(arr);  // → [1, 2, 3, 5]
```

### `eliminar` omitido — eliminar hasta el final

```js
const arr = ["a", "b", "c", "d"];

const cola = arr.splice(2);
console.log(cola); // → ["c", "d"]
console.log(arr);  // → ["a", "b"]
```

## Recetas comunes

```js
// Eliminar un elemento por valor (primera ocurrencia)
function eliminarPorValor(arr, valor) {
  const idx = arr.indexOf(valor);
  if (idx !== -1) arr.splice(idx, 1);
}

// Insertar en posición ordenada
function insertarOrdenado(arr, val) {
  const idx = arr.findIndex(x => x > val);
  arr.splice(idx === -1 ? arr.length : idx, 0, val);
}

// Reemplazar un elemento por valor
function reemplazar(arr, viejo, nuevo) {
  const idx = arr.indexOf(viejo);
  if (idx !== -1) arr.splice(idx, 1, nuevo);
}
```

## Alternativa inmutable: `toSpliced` (ES2023)

`arr.toSpliced(inicio, eliminar, ...insertar)` retorna un **nuevo array** con la operación aplicada sin mutar el original:

```js
const original = [1, 2, 3, 4];
const nuevo = original.toSpliced(1, 1, 99);

console.log(nuevo);    // → [1, 99, 3, 4]
console.log(original); // → [1, 2, 3, 4]  (sin cambios)
```

> [!tip] Buenas prácticas
> - Preferir `toSpliced` en código funcional o con estado inmutable (ES2023+).
> - Capturar el retorno de `splice` cuando se necesiten los elementos eliminados.
> - Para operaciones simples de inicio/fin, `push`/`pop`/`shift`/`unshift` son más legibles y tienen mejor semántica expresiva.

> [!warning] Errores comunes
> - Olvidar que `splice` **muta** el array: `const copia = arr.splice(...)` no es una copia del array; es el array de elementos eliminados.
> - Llamar `arr.splice(idx)` con un solo argumento elimina todos los elementos desde `idx` hasta el final — puede vaciar parcialmente el array sin querer.
> - Usar `splice` dentro de un bucle `for` con índice fijo: al eliminar un elemento, los índices siguientes se desplazan. Iterar desde el final hacia el principio o usar índice dinámico.

## Cómo funciona por dentro

Tras la operación, el motor desplaza los elementos restantes para llenar el hueco (si se eliminó más de lo que se insertó) o para hacer espacio (si se insertó más). Esto requiere reasignar propiedades numéricas: costo O(n) respecto al número de elementos afectados por el desplazamiento. `length` se actualiza al final.

## Notas relacionadas

- [[06 slice]]
- [[03 push y pop]]
- [[04 shift y unshift]]
- [[index|Arrays]]
