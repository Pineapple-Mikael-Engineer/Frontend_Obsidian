---
title: "flatMap"
aliases:
  - Array flatMap
  - array.flatMap
tags:
  - javascript
  - api/metodo
  - arrays
draft: false
order: 11
---

# flatMap

> [!definicion]
> `Array.prototype.flatMap` aplica un callback a cada elemento y aplana el resultado **un nivel**. Es equivalente a `map` seguido de `flat(1)`, pero más eficiente porque realiza la operación en una sola pasada sobre el array. El callback puede devolver un valor escalar (se incluye directamente) o un array (sus elementos se incorporan al resultado).

```js
const oraciones = ["hola mundo", "foo bar baz"];

// Con map + flat:
oraciones.map((s) => s.split(" ")).flat();
// ["hola", "mundo", "foo", "bar", "baz"]

// Con flatMap (equivalente, más eficiente):
oraciones.flatMap((s) => s.split(" "));
// ["hola", "mundo", "foo", "bar", "baz"]
```

## Firma del método

| Parámetro del callback | Tipo | Descripción |
|---|---|---|
| `elemento` | any | Valor del elemento actual |
| `índice` | number | Posición (0-based) |
| `array` | Array | Referencia al array original |

| Aspecto | Detalle |
|---|---|
| Valor de retorno del método | Nuevo `Array` con el resultado aplanado un nivel |
| Profundidad de aplanado | Siempre 1 (fijo) |
| Callback devuelve array | Sus elementos se incorporan al resultado |
| Callback devuelve valor | El valor se incorpora directamente |
| Callback devuelve `[]` | No añade nada (útil para filtrar) |
| Muta el array | No |
| Argumento `thisArg` | Opcional |

## Ejemplos

### Expandir etiquetas de una lista de posts

```js
const posts = [
  { título: "JS Basics", tags: ["js", "web"] },
  { título: "CSS Grid", tags: ["css", "layout", "web"] },
  { título: "Node.js", tags: ["js", "backend"] },
];

const todasLasEtiquetas = posts.flatMap((p) => p.tags);
// ["js", "web", "css", "layout", "web", "js", "backend"]

// Para las únicas:
const etiquetasÚnicas = [...new Set(posts.flatMap((p) => p.tags))];
// ["js", "web", "css", "layout", "backend"]
```

### Tokenizar texto en palabras

```js
const párrafos = [
  "El cielo es azul",
  "La hierba es verde",
];

const palabras = párrafos.flatMap((p) =>
  p.toLowerCase().split(/\s+/).filter((w) => w.length > 2)
);
// ["cielo", "azul", "hierba", "verde"]
```

### Generar rangos o series a partir de valores

```js
const cantidades = [2, 3, 1];

const índices = cantidades.flatMap((n, i) =>
  Array.from({ length: n }, (_, j) => `item-${i}-${j}`)
);
// ["item-0-0", "item-0-1", "item-1-0", "item-1-1", "item-1-2", "item-2-0"]
```

### Relaciones uno a muchos (JOIN conceptual)

```js
const pedidos = [
  { id: 1, productos: ["Laptop", "Mouse"] },
  { id: 2, productos: ["Teclado"] },
  { id: 3, productos: ["Monitor", "Webcam", "Altavoces"] },
];

const líneasDePedido = pedidos.flatMap(({ id, productos }) =>
  productos.map((producto) => ({ pedidoId: id, producto }))
);
// [{pedidoId: 1, producto: "Laptop"}, {pedidoId: 1, producto: "Mouse"}, ...]
```

## El truco de filtrar y transformar con `flatMap`

Devolver un array vacío `[]` es equivalente a excluir el elemento del resultado. Esto permite **filtrar y transformar simultáneamente** en una sola pasada:

```js
const datos = [1, -2, 3, -4, 5, -6];

// filter + map: dos pasadas
const positivos = datos.filter((n) => n > 0).map((n) => n * 10);
// [10, 30, 50]

// flatMap: una sola pasada
const positivosConFlatMap = datos.flatMap((n) => n > 0 ? [n * 10] : []);
// [10, 30, 50]
```

Este patrón es especialmente útil cuando la decisión de incluir un elemento depende de la transformación misma:

```js
const textos = ["", "hola", "  ", "mundo", ""];

const limpios = textos.flatMap((t) => {
  const trimmed = t.trim();
  return trimmed ? [trimmed] : [];
});
// ["hola", "mundo"]
```

## Cuándo usar `flatMap` vs `map + flat`

```js
const arr = [[1, 2], [3, 4]];

// Para aplanar más de un nivel, flatMap no es suficiente:
arr.flatMap((x) => x);        // [1, 2, 3, 4] — aplanado 1 nivel
arr.map((x) => x).flat(2);    // necesitas flat(n) para más niveles

// Para un nivel, flatMap es preferible por eficiencia:
arr.map((x) => x.map(n => n * 2)).flat();     // dos pasadas
arr.flatMap((x) => x.map(n => n * 2));        // una pasada
```

`flatMap` siempre aplana exactamente un nivel. Para profundidades mayores, combinar `map` con `flat(n)` explícito.

## Cómo funciona por dentro

`flatMap` itera el array original desde el índice `0`. Para cada elemento, invoca el callback y examina el resultado: si es un array, sus elementos se empujan al array de resultado; si no es un array, el valor mismo se empuja. En términos de implementación, es equivalente a `map` + `flat(1)` pero sin crear el array intermedio de la fase `map`.

```js
// Implementación conceptual
Array.prototype.flatMapPropio = function(callback, thisArg) {
  const resultado = [];
  for (let i = 0; i < this.length; i++) {
    if (i in this) {
      const val = callback.call(thisArg, this[i], i, this);
      if (Array.isArray(val)) {
        resultado.push(...val);    // aplana un nivel
      } else {
        resultado.push(val);
      }
    }
  }
  return resultado;
};
```

La eficiencia de `flatMap` sobre `map().flat()` proviene de evitar alojar el array intermedio que `map` crearía.

> [!tip] Buenas prácticas
> - Usar `flatMap` cuando el callback naturalmente devuelve arrays (expansión, one-to-many) y quieres un resultado plano.
> - El patrón `flatMap(x => condición ? [transform(x)] : [])` es una forma concisa de combinar filter y map — pero solo cuando la transformación y el filtro están relacionados. Si son independientes, `filter().map()` puede ser más legible.
> - Para aplanar más de un nivel siempre usar `map(...).flat(n)` — `flatMap` es estrictamente de un nivel.
> - `flatMap` es ES2019. Verificar compatibilidad para entornos legacy.

> [!warning] Errores comunes
> - **Esperar que aplane más de un nivel:** `flatMap` solo aplana un nivel, igual que `flat(1)`. Para más profundidad, usar `map` + `flat(n)`.
> - **Devolver un objeto cuando se esperaba un array:** si el callback devuelve `{ a: 1 }` en lugar de `[{ a: 1 }]`, el objeto se incluye directamente (no se aplana — los objetos no son arrays). Esto es correcto pero puede no ser lo esperado.
> - **Olvidar que devolver `undefined` implícitamente añade `undefined`:** si el callback no devuelve nada en alguna rama, ese `undefined` se añade al resultado. Usar `[]` para excluir.
> - **Confundir con `flatten` de otras librerías:** en Lodash `_.flatMap` tiene comportamientos similares pero con diferencias en cómo maneja valores no-array.
> - **Rendimiento en arrays muy grandes:** aunque `flatMap` es más eficiente que `map().flat()`, crear muchos arrays pequeños en el callback (`[x]`) tiene coste. Para millones de elementos, un bucle con `push` puede ser más rápido.

## Notas relacionadas

- [[index|Métodos de Array Funcionales — Índice]]
- [[02 map]]
- [[03 filter]]
- [[10 flat]]
- [[12 Encadenamiento de Métodos]]
