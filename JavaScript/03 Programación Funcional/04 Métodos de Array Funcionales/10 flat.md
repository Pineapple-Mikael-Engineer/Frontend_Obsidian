---
title: "flat"
aliases:
  - Array flat
  - array.flat
tags:
  - javascript
  - api/metodo
  - arrays
draft: false
---

# flat

> [!definicion]
> `Array.prototype.flat` devuelve un **nuevo array** con los sub-arrays concatenados de forma recursiva hasta la profundidad indicada. No muta el array original. Además, elimina los huecos (elementos vacíos) en cada nivel que aplana.

```js
const anidado = [1, [2, 3], [4, [5, 6]]];

anidado.flat();
// [1, 2, 3, 4, [5, 6]]  ← profundidad 1 (por defecto)

anidado.flat(2);
// [1, 2, 3, 4, 5, 6]    ← profundidad 2

anidado.flat(Infinity);
// [1, 2, 3, 4, 5, 6]    ← profundidad ilimitada
```

## Firma del método

| Parámetro | Tipo | Descripción |
|---|---|---|
| `profundidad` | number | Niveles de anidamiento a aplanar. Por defecto: `1` |

| Aspecto | Detalle |
|---|---|
| Valor de retorno | Nuevo `Array` aplanado |
| `profundidad` por defecto | `1` |
| `flat(Infinity)` | Aplana a cualquier profundidad |
| Muta el array | No |
| Huecos en arrays sparse | Los elimina en cada nivel aplanado |

## Ejemplos

### Aplanar resultados de múltiples consultas

```js
const resultados = [
  [{ id: 1 }, { id: 2 }],
  [{ id: 3 }],
  [{ id: 4 }, { id: 5 }, { id: 6 }],
];

const todos = resultados.flat();
// [{id:1}, {id:2}, {id:3}, {id:4}, {id:5}, {id:6}]
```

### Distinguir niveles de profundidad

```js
const datos = [1, [2, [3, [4, [5]]]]];

datos.flat(0);   // [1, [2, [3, [4, [5]]]]]  — sin cambio
datos.flat(1);   // [1, 2, [3, [4, [5]]]]
datos.flat(2);   // [1, 2, 3, [4, [5]]]
datos.flat(3);   // [1, 2, 3, 4, [5]]
datos.flat(4);   // [1, 2, 3, 4, 5]
datos.flat(Infinity); // [1, 2, 3, 4, 5]
```

### Aplanar y eliminar huecos

```js
const conHuecos = [1, , 2, [3, , 4], , 5];
conHuecos.flat();
// [1, 2, 3, 4, 5]  ← los huecos desaparecen en el nivel aplanado
```

Este comportamiento lo diferencia de `concat` y del operador spread, que también eliminan huecos al aplanar pero de forma más verbosa.

### Normalizar estructura de árbol a lista plana

```js
// Árbol de categorías
const categorías = [
  { nombre: "Electrónica", subcategorías: [
    { nombre: "Teléfonos" },
    { nombre: "Laptops" },
  ]},
  { nombre: "Ropa", subcategorías: [
    { nombre: "Camisas" },
  ]},
];

const todasLasCategorías = [
  ...categorías.map((c) => c.nombre),
  ...categorías.map((c) => c.subcategorías.map((s) => s.nombre)).flat(),
];
// ["Electrónica", "Ropa", "Teléfonos", "Laptops", "Camisas"]

// Más compacto con flatMap:
const nombres = categorías.flatMap((c) => [c.nombre, ...c.subcategorías.map((s) => s.nombre)]);
```

## Recetas comunes

### Combinar múltiples arrays de resultados paginados

```js
const páginas = await Promise.all(
  [1, 2, 3].map((página) => fetch(`/api/items?page=${página}`).then((r) => r.json()))
);
const items = páginas.flat();
```

### Aplanar un array de arrays de profundidad desconocida

```js
const aplanarTodo = (arr) => arr.flat(Infinity);

aplanarTodo([1, [2, [3, [4, [5, [6]]]]]]);
// [1, 2, 3, 4, 5, 6]
```

### Convertir estructura de árbol (profundidad conocida) en lista

```js
// Árbol de dos niveles → lista plana
const temas = [
  ["JavaScript", ["Variables", "Funciones", "Objetos"]],
  ["CSS", ["Selectores", "Flexbox", "Grid"]],
];

const temario = temas.flat(1).flat(); // o temas.flat(2)
// ["JavaScript", "Variables", "Funciones", "Objetos", "CSS", "Selectores", "Flexbox", "Grid"]
```

## Comparación con `concat` y spread

Antes de ES2019 (cuando se introdujo `flat`), el equivalente más común para aplanar un nivel era:

```js
const anidado = [[1, 2], [3, 4], [5, 6]];

// Equivalentes a flat(1):
[].concat(...anidado);          // [1, 2, 3, 4, 5, 6]
[].concat.apply([], anidado);   // [1, 2, 3, 4, 5, 6]

// Con flat (ES2019):
anidado.flat();                 // [1, 2, 3, 4, 5, 6]
```

`flat()` es más legible, soporta múltiples niveles y maneja correctamente los huecos. La alternativa con spread tiene un límite de argumentos (`Function.apply` puede fallar con arrays muy grandes por el límite de la pila de llamadas).

```js
// Problema con spread en arrays muy grandes:
const grande = Array.from({ length: 1_000_000 }, (_, i) => [i]);
[].concat(...grande);   // Puede lanzar RangeError: Maximum call stack size exceeded
grande.flat();          // Funciona sin problemas
```

## Cómo funciona por dentro

`flat` crea un nuevo array vacío y recorre el array original elemento por elemento. Si un elemento es a su vez un array y la profundidad restante es mayor que `0`, sus elementos se añaden al resultado de forma recursiva (decrementando la profundidad). Si el elemento no es un array (o la profundidad ya es `0`), se añade directamente. Los huecos se omiten en cada nivel procesado.

```js
// Implementación conceptual (simplificada)
Array.prototype.flatPropio = function(profundidad = 1) {
  return this.reduce((acc, elem) => {
    if (Array.isArray(elem) && profundidad > 0) {
      acc.push(...elem.flatPropio(profundidad - 1));
    } else if (elem !== undefined || !(this.indexOf(elem) in this)) {
      // simplificado — en realidad verifica propias y huecos
      acc.push(elem);
    }
    return acc;
  }, []);
};
```

> [!tip] Buenas prácticas
> - Usar `flat(1)` (o simplemente `flat()`) para el caso más común de aplanar un nivel — es explícito sobre la profundidad.
> - Usar `flat(Infinity)` solo cuando la profundidad es genuinamente desconocida o variable; para profundidades conocidas, ser explícito.
> - Combinar `flat` con `map` solo cuando necesitas más de un nivel de aplanado; para un nivel, `flatMap` es más eficiente.
> - Recordar que `flat` también elimina huecos — si los huecos son significativos en tu datos, esto puede ser un efecto no deseado.

> [!warning] Errores comunes
> - **Asumir que `flat()` aplana a cualquier profundidad:** por defecto solo aplana un nivel. Para profundidad total, pasar `Infinity`.
> - **Esperar que `flat` mute el original:** devuelve un nuevo array; el original no cambia.
> - **No conocer la eliminación de huecos:** `flat` elimina slots vacíos en arrays sparse en el nivel que aplana. Si los huecos tienen significado, esto es un bug.
> - **Usar `[].concat(...arr)` en arrays grandes:** puede causar desbordamiento de pila. Usar `flat(1)` en su lugar.
> - **`flat` no existe en entornos pre-ES2019:** verificar compatibilidad o usar polyfill/`reduce` como alternativa.

## Notas relacionadas

- [[index|Métodos de Array Funcionales — Índice]]
- [[11 flatMap]]
- [[02 map]]
- [[12 Encadenamiento de Métodos]]
