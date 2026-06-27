---
title: Array.prototype.concat — Unir arrays en uno nuevo
aliases:
  - concat
  - Array.concat
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
order: 7
---

# Array.prototype.concat

> [!definicion]
> `arr.concat(...valores)` retorna un **nuevo array** que contiene los elementos de `arr` seguidos de los elementos de cada `valor`. Si un valor es un array (o un objeto con `Symbol.isConcatSpreadable = true`), se aplana un nivel. No muta ninguno de los arrays involucrados.

## Firma

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `...valores` | `any` (variádico) | Arrays u otros valores a concatenar |

**Retorna:** nuevo array. **No muta.**

## Ejemplos base

```js
const a = [1, 2, 3];
const b = [4, 5];

a.concat(b);         // → [1, 2, 3, 4, 5]
a.concat(b, [6, 7]); // → [1, 2, 3, 4, 5, 6, 7]
a.concat(4, 5);      // → [1, 2, 3, 4, 5]   (valores no-array se añaden directamente)

// El original no muta
console.log(a); // → [1, 2, 3]
console.log(b); // → [4, 5]
```

## Aplanamiento de un solo nivel

`concat` aplana solo un nivel de arrays. Arrays anidados más profundos se insertan tal cual:

```js
[1].concat([2, [3, 4]]); // → [1, 2, [3, 4]]  ← el [3,4] no se aplana
[1].concat([[2, 3]]);    // → [1, [2, 3]]       ← tampoco
```

Para aplanamiento más profundo, ver `flat`.

## `Symbol.isConcatSpreadable`

Un objeto con `[Symbol.isConcatSpreadable] = true` y una propiedad `length` con propiedades numéricas se aplana en `concat` como si fuera un array:

```js
const arrayLike = { 0: "x", 1: "y", length: 2, [Symbol.isConcatSpreadable]: true };
[1, 2].concat(arrayLike); // → [1, 2, "x", "y"]
```

Un array normal tiene `Symbol.isConcatSpreadable` heredado como `true` en su prototipo. Se puede desactivar:

```js
const arr = [3, 4];
arr[Symbol.isConcatSpreadable] = false;
[1, 2].concat(arr); // → [1, 2, [3, 4]]  ← arr se inserta como elemento único
```

## `concat` vs spread

| Aspecto | `arr.concat(a, b)` | `[...arr, ...a, ...b]` |
|---------|--------------------|------------------------|
| Legibilidad con muchos arrays | mejor | más ruidoso |
| Con arrays dinámicos | `arr.concat(...lista)` | `[...arr, ...lista.flat()]` |
| Arrays anidados | aplana 1 nivel | aplana 1 nivel |
| Objetos con isConcatSpreadable | sí los aplana | no los aplana |

El spread es más idiomático para 2–3 arrays fijos. `concat` resulta más claro cuando se unen múltiples fuentes dinámicas.

## Recetas comunes

```js
// Combinar resultados de múltiples páginas
function combinarPaginas(...paginas) {
  return [].concat(...paginas);
}

const todo = combinarPaginas([1, 2], [3, 4], [5, 6]);
// → [1, 2, 3, 4, 5, 6]

// Añadir elementos a un array sin mutarlo
const base = [1, 2, 3];
const ampliado = base.concat(4, 5);
// base sigue siendo [1, 2, 3]

// Copia superficial (equivalente a slice())
const copia = [].concat(original);
```

> [!tip] Buenas prácticas
> - Usar `concat` cuando se unen más de 2–3 arrays o cuando los arrays provienen de una lista dinámica.
> - Preferir spread `[...a, ...b]` para uniones simples de literales — más legible.
> - `[].concat(...arrayDeArrays)` aplana un nivel de un array de arrays (alternativa a `flat(1)` para compatibilidad).

> [!warning] Errores comunes
> - Esperar que `concat` aplana más de un nivel. `[1].concat([[2, [3]]])` → `[1, [2, [3]]]` (solo aplana el primer nivel del argumento).
> - Mutar el receptor pensando que `concat` lo modifica: `arr.concat(b)` nunca toca `arr`.
> - Usar `concat` dentro de bucles para acumular: O(n²) porque crea un nuevo array en cada iteración. Preferir `push` en bucle o `flat`/`flatMap` para transformaciones acumulativas.

## Notas relacionadas

- [[06 slice]]
- [[03 push y pop]]
- [[index|Arrays]]
