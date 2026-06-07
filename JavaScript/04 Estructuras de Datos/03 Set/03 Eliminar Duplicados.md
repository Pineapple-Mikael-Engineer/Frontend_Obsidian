---
title: Set — Eliminar Duplicados
aliases:
  - deduplicar array
  - eliminar duplicados con Set
  - unique array javascript
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: Set
tipo: patron
draft: false
---

# Set — Eliminar Duplicados

> [!definicion]
> El patrón `[...new Set(array)]` es la forma idiomática de eliminar valores duplicados de un array en JavaScript. Funciona en O(n), preserva el orden de primera aparición y es de una sola expresión. Para primitivos es directo; para objetos, la comparación es por referencia — se necesita una estrategia diferente si la igualdad es por contenido.

```js
const nums = [1, 3, 2, 3, 1, 4, 2];
const unicos = [...new Set(nums)];
console.log(unicos); // → [1, 3, 2, 4]  (orden de primera aparición)
```

## La receta canónica

```js
// Spread (más conciso)
const unicos = [...new Set(arr)];

// Array.from (más explícito, acepta segundo argumento de mapeo)
const unicos2 = Array.from(new Set(arr));

// Con mapeo en un paso
const unicosDoblados = Array.from(new Set(arr), x => x * 2);
```

## Funciona con primitivos — no con objetos por valor

`Set` compara por SameValueZero. Para primitivos (números, strings, booleans, `null`, `undefined`, `NaN`, Symbols), dos valores con el mismo contenido son el mismo valor. Para objetos, la comparación es por referencia — dos objetos distintos en memoria son siempre entradas diferentes aunque tengan el mismo contenido.

```js
// Primitivos — funciona como se espera
const strs = ["a", "b", "a", "c", "b"];
console.log([...new Set(strs)]); // → ["a", "b", "c"]

// NaN también se deduplica correctamente
const conNaN = [NaN, 1, NaN, 2, NaN];
console.log([...new Set(conNaN)]); // → [NaN, 1, 2]

// Objetos — NO deduplica por contenido
const objs = [{ id: 1 }, { id: 2 }, { id: 1 }];
console.log(new Set(objs).size); // → 3  (tres referencias distintas)
```

## Deduplicar objetos por propiedad

Cuando lo que se quiere es eliminar objetos con el mismo valor en una propiedad clave, usar `Map` con esa propiedad como clave — el `Map` conserva la última ocurrencia (o la primera si se invierte):

```js
const usuarios = [
  { id: 1, nombre: "Ana" },
  { id: 2, nombre: "Luis" },
  { id: 1, nombre: "Ana (actualizado)" },
  { id: 3, nombre: "Marta" },
];

// Conserva la última ocurrencia por id
const unicos = [...new Map(usuarios.map(u => [u.id, u])).values()];
console.log(unicos);
// → [{id:1, nombre:"Ana (actualizado)"}, {id:2,...}, {id:3,...}]

// Conserva la primera ocurrencia por id
const primeraPorId = new Map();
for (const u of usuarios) {
  if (!primeraPorId.has(u.id)) primeraPorId.set(u.id, u);
}
const unicosPrimeros = [...primeraPorId.values()];
console.log(unicosPrimeros);
// → [{id:1, nombre:"Ana"}, {id:2,...}, {id:3,...}]
```

## Preservación de orden

`Set` garantiza el orden de primera inserción. Al convertir un array a Set y volver a array, los elementos aparecen en el orden en que se vieron por primera vez.

```js
const letras = ["c", "a", "b", "a", "c", "d", "b"];
const unicas = [...new Set(letras)];
console.log(unicas); // → ["c", "a", "b", "d"]  (primer orden de aparición)

// Contraste: sort eliminaría el orden original
const ordenadas = [...new Set(letras)].sort();
console.log(ordenadas); // → ["a", "b", "c", "d"]
```

## Rendimiento

| Método | Complejidad | Nota |
|--------|-------------|------|
| `new Set(arr)` + spread | O(n) | Estándar para duplicados |
| `filter` + `indexOf` | O(n²) | Lento en arrays grandes |
| `filter` + objeto como mapa | O(n) | Solo strings como claves |
| `reduce` acumulando | O(n²) | Antipatrón frecuente |

```js
const arr = Array.from({ length: 10000 }, (_, i) => i % 1000);

// O(n) — recomendado
const r1 = [...new Set(arr)];

// O(n²) — evitar en arrays grandes
const r2 = arr.filter((v, i) => arr.indexOf(v) === i);
```

## Casos de uso reales

```js
// Tags únicos de múltiples posts
const posts = [
  { tags: ["js", "web"] },
  { tags: ["css", "web"] },
  { tags: ["js", "node"] },
];
const todosLosTags = posts.flatMap(p => p.tags);
const tagsUnicos = [...new Set(todosLosTags)];
console.log(tagsUnicos); // → ["js", "web", "css", "node"]

// IDs únicos de una lista de eventos
const eventos = [
  { userId: 1 }, { userId: 2 }, { userId: 1 }, { userId: 3 }
];
const userIds = [...new Set(eventos.map(e => e.userId))];
console.log(userIds); // → [1, 2, 3]

// Valores de un campo en un array de registros
const productos = [
  { categoria: "ropa" }, { categoria: "tech" },
  { categoria: "ropa" }, { categoria: "hogar" }
];
const categorias = [...new Set(productos.map(p => p.categoria))];
console.log(categorias); // → ["ropa", "tech", "hogar"]
```

## Eliminar duplicados en strings (caracteres únicos)

```js
const unicos = [...new Set("javascript")].join("");
console.log(unicos); // → "javscrpt"  (caracteres únicos en orden de aparición)

// Comprobar si un string tiene todos los caracteres únicos
function todosUnicos(str) {
  return new Set(str).size === str.length;
}
console.log(todosUnicos("abcde")); // → true
console.log(todosUnicos("abcda")); // → false
```

## Cómo funciona por dentro

`new Set(arr)` itera el array una vez: por cada elemento calcula el hash con SameValueZero y lo inserta en el bucket correspondiente. Si ya existe, lo ignora. Al final, el Set contiene exactamente los valores únicos en orden de primera inserción. La conversión `[...set]` recorre la lista enlazada interna en orden de inserción — también O(n). El costo total es O(n) tanto en tiempo como en espacio.

> [!tip] Buenas prácticas
> - Para primitivos, `[...new Set(arr)]` es la opción por defecto — concisa, correcta y eficiente.
> - Para objetos, definir explícitamente qué significa "duplicado" (por qué propiedad) y usar la estrategia Map correspondiente.
> - Si solo se necesita contar únicos sin convertir a array, `new Set(arr).size` evita la conversión intermedia.

> [!warning] Errores comunes
> - **Aplicar la receta a objetos y esperar deduplicación por contenido:** `[...new Set([{a:1},{a:1}])]` produce dos elementos — los objetos son referencias distintas.
> - **Suponer que los booleanos se deduplicarán con sus equivalentes coercitivos:** `Set` no hace coerción de tipos. `true` y `1` son valores distintos en un Set.
> - **Olvidar el spread al convertir:** `new Set(arr)` devuelve un Set, no un array. Sin `[...]` o `Array.from`, no se puede indexar ni usar métodos de Array.

## Notas relacionadas

- [[03 Set/index|Set — índice]]
- [[01 Creación y add]]
- [[02 has, delete, clear, size]]
- [[02 Map/index|Map — índice]]
