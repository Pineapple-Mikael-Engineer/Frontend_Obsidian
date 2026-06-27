---
title: Array.prototype.slice — Extraer subarreglo sin mutar
aliases:
  - slice
  - Array.slice
tags:
  - javascript
  - api/metodo
  - arrays
objeto: Array
tipo: metodo
retorna: Array
muta: false
asincrono: false
draft: false
order: 6
---

# Array.prototype.slice

> [!definicion]
> `arr.slice(inicio?, fin?)` retorna un **nuevo array** con los elementos desde el índice `inicio` hasta `fin` (exclusivo). No muta el original. Admite índices negativos (cuentan desde el final). Sin argumentos, produce una copia superficial completa del array.

## Firma

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `inicio` | `number` (opcional) | Índice de inicio, inclusivo. Por defecto `0`. Negativo: desde el final |
| `fin` | `number` (opcional) | Índice de fin, exclusivo. Por defecto `arr.length`. Negativo: desde el final |

**Retorna:** nuevo array (copia superficial del rango). **No muta.**

## Ejemplos base

```js
const arr = ["a", "b", "c", "d", "e"];

arr.slice(1, 3);  // → ["b", "c"]         (índices 1 y 2, fin excluido)
arr.slice(2);     // → ["c", "d", "e"]    (desde índice 2 hasta el final)
arr.slice(-2);    // → ["d", "e"]         (últimos 2)
arr.slice(1, -1); // → ["b", "c", "d"]   (desde 1 hasta el penúltimo)
arr.slice();      // → ["a", "b", "c", "d", "e"]  (copia superficial completa)
```

## Índices negativos

El índice negativo `i` equivale a `arr.length + i`. Si el índice resultante es menor que 0, se trata como 0.

```js
const arr = [10, 20, 30, 40, 50];

arr.slice(-3);     // → [30, 40, 50]   (-3 → índice 2)
arr.slice(-3, -1); // → [30, 40]       (-3 → 2, -1 → 4)
arr.slice(-100);   // → [10, 20, 30, 40, 50]  (clampea a 0)
```

## Recetas comunes

### Copia superficial

```js
const original = [1, 2, 3];
const copia = original.slice();

copia.push(4);
console.log(original); // → [1, 2, 3]  (sin cambios)
console.log(copia);    // → [1, 2, 3, 4]
```

### Paginación

```js
function paginar(arr, pagina, tamano) {
  return arr.slice(pagina * tamano, (pagina + 1) * tamano);
}

const datos = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
paginar(datos, 0, 3); // → [1, 2, 3]
paginar(datos, 1, 3); // → [4, 5, 6]
paginar(datos, 2, 3); // → [7, 8, 9]
paginar(datos, 3, 3); // → [10]
```

### Convertir `arguments` a array (legado)

```js
function suma() {
  const args = Array.prototype.slice.call(arguments);
  return args.reduce((acc, x) => acc + x, 0);
}
suma(1, 2, 3); // → 6
```

En código moderno, preferir parámetros rest: `function suma(...args)`.

### Eliminar el primer o último elemento de forma inmutable

```js
const arr = [1, 2, 3, 4];

const sinPrimero = arr.slice(1);     // → [2, 3, 4]
const sinUltimo  = arr.slice(0, -1); // → [1, 2, 3]
```

## `slice` vs `splice`

| Aspecto | `slice` | `splice` |
|---------|---------|----------|
| Muta el original | no | **sí** |
| Retorna | nuevo array con el rango | array de elementos eliminados |
| Puede insertar | no | sí |
| Puede eliminar | no (solo lee) | sí |
| `fin` exclusivo | sí | no aplica (segundo param es conteo) |

> [!tip] Buenas prácticas
> - Preferir `slice()` sobre spread `[...arr]` cuando se quiere documentar explícitamente que se hace una copia.
> - Para rangos de paginación o ventanas deslizantes, `slice` es la herramienta directa.
> - `slice` produce una **copia superficial**: los objetos dentro del array comparten referencia con el original. Si se necesita copia profunda, usar `structuredClone`.

> [!warning] Errores comunes
> - Confundir el segundo parámetro de `slice` (índice exclusivo) con el de `splice` (conteo de elementos). `arr.slice(1, 3)` extrae 2 elementos; `arr.splice(1, 3)` elimina 3.
> - Esperar que `slice` clone objetos anidados. `arr.slice()` es superficial: `arr[0].x = 1` afecta también a la copia.
> - Usar `slice(0)` y `slice()` son equivalentes — algunos desarrolladores añaden el `0` innecesariamente pensando que es más explícito.

## Notas relacionadas

- [[05 splice]]
- [[07 concat]]
- [[02 Acceso por Índice y length]]
- [[index|Arrays]]
