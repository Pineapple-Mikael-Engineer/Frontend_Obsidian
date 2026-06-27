---
title: Set — Creación y add
aliases:
  - new Set
  - set.add
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: Set
tipo: constructor-metodo
retorna: Set
muta: true
asincrono: false
draft: false
order: 1
---

# Set — Creación y `add`

> [!definicion]
> `new Set()` crea un conjunto vacío. `new Set(iterable)` crea un conjunto a partir de cualquier iterable — array, string, otro Set — eliminando duplicados automáticamente. `set.add(valor)` añade el valor si no está presente (usando SameValueZero) y devuelve el propio Set, permitiendo encadenamiento. Los duplicados se ignoran silenciosamente.

```js
// Conjunto vacío
const vacio = new Set();

// Desde array — duplicados eliminados
const nums = new Set([1, 2, 3, 2, 1]);
console.log(nums.size);   // → 3
console.log([...nums]);   // → [1, 2, 3]

// add chainable
const s = new Set();
s.add("a").add("b").add("a").add("c");
console.log([...s]);      // → ["a", "b", "c"]
```

## Firmas

```js
new Set()                  // conjunto vacío
new Set(iterable)          // desde array, string, Map.keys(), otro Set…
set.add(valor)             // → el propio Set (chainable)
```

## `new Set()` — conjunto vacío

Un Set vacío empieza sin entradas. El uso habitual es crearlo vacío y rellenarlo con `add` o en un bucle.

```js
const tags = new Set();
["js", "css", "js", "html", "css"].forEach(t => tags.add(t));
console.log([...tags]); // → ["js", "css", "html"]
```

## `new Set(iterable)` — desde iterable

Cualquier iterable es válido: arrays, strings (separa por carácter), otro Set, `Map.keys()`, generadores, `NodeList`…

```js
// Desde string — cada carácter es un valor
const letras = new Set("abracadabra");
console.log([...letras]); // → ["a", "b", "r", "c", "d"]

// Desde otro Set — copia efectiva
const original = new Set([1, 2, 3]);
const copia = new Set(original);
copia.add(4);
console.log(original.size); // → 3  (no muta el original)

// Desde Map.keys()
const m = new Map([["x", 1], ["y", 2], ["x", 3]]);
const claves = new Set(m.keys());
console.log([...claves]); // → ["x", "y"]
```

## `add` — añadir valores

`add` compara con SameValueZero antes de insertar. Si el valor ya está, no hace nada y devuelve el Set sin modificar. El valor de retorno es siempre el propio Set — útil para inicialización fluida.

```js
const s = new Set([1, 2, 3]);

s.add(2);           // ya existe — ignorado
s.add(4);           // nuevo
console.log([...s]); // → [1, 2, 3, 4]

// Chainable
new Set().add(10).add(20).add(30); // → Set {10, 20, 30}
```

## SameValueZero — qué cuenta como duplicado

| Comparación | Resultado |
|-------------|-----------|
| `1` y `1` | iguales |
| `"a"` y `"a"` | iguales |
| `NaN` y `NaN` | iguales (a diferencia de `===`) |
| `+0` y `-0` | iguales (normalizados) |
| `{}` y `{}` | distintos (referencia diferente) |
| misma variable objeto | iguales (misma referencia) |

```js
const s = new Set();

// NaN se trata como igual a sí mismo
s.add(NaN);
s.add(NaN);
console.log(s.size); // → 1

// +0 y -0 son la misma entrada
s.add(+0);
s.add(-0);
console.log(s.size); // → 2  (NaN + cero)

// Objetos por referencia
s.add({});
s.add({});           // objeto distinto aunque tenga el mismo contenido
console.log(s.size); // → 4
```

## Objetos en Set — comparación por referencia

Cuando se añaden objetos, la igualdad se comprueba por identidad de referencia, no por contenido. Dos objetos literales `{}` son siempre distintos aunque tengan las mismas propiedades.

```js
const obj = { id: 1 };
const s = new Set();

s.add(obj);
s.add(obj);           // misma referencia — ignorado
s.add({ id: 1 });     // referencia nueva — entra

console.log(s.size);  // → 2
```

## Conversión a/desde array

```js
const arr = [1, 2, 3, 2, 1];

// Array → Set (elimina duplicados)
const set = new Set(arr);

// Set → Array (dos formas equivalentes)
const a1 = [...set];           // spread
const a2 = Array.from(set);   // Array.from

console.log(a1); // → [1, 2, 3]
console.log(a2); // → [1, 2, 3]
```

## Recetas comunes

### Eliminar duplicados de un array (primitivos)

```js
const tags = ["js", "css", "js", "html", "css", "html", "js"];
const unicos = [...new Set(tags)];
console.log(unicos); // → ["js", "css", "html"]
```

### Construir un Set incremental desde una API

```js
const vistos = new Set();

function procesarResultados(items) {
  for (const item of items) {
    if (vistos.has(item.id)) continue;
    vistos.add(item.id);
    // procesar item por primera vez
  }
}
```

### Unión rápida de dos arrays sin duplicados

```js
const a = [1, 2, 3];
const b = [2, 3, 4, 5];
const union = [...new Set([...a, ...b])];
console.log(union); // → [1, 2, 3, 4, 5]
```

## Cómo funciona por dentro

`Set` implementa internamente una tabla hash donde cada valor actúa como su propia clave. La comparación usa SameValueZero: el motor normaliza `NaN` a una clave especial canónica y `−0` a `+0` antes de calcular el hash. El orden de inserción se mantiene con una lista enlazada interna que une los buckets en secuencia temporal — `add` añade al final de esa lista. Por eso la iteración siempre respeta el orden de primera inserción.

> [!tip] Buenas prácticas
> - Usar `new Set(array)` para deduplicar arrays de primitivos — es O(n) y de una línea.
> - Para conjuntos que se construyen incrementalmente, crear vacío y hacer `add` en el bucle — más legible que acumular en un array y deduplicar al final.
> - Al convertir de vuelta a array, preferir spread `[...set]` para concisión o `Array.from(set)` cuando necesitas el segundo argumento de mapeo (`Array.from(set, x => x * 2)`).

> [!warning] Errores comunes
> - **Deduplicar objetos por valor:** `new Set([{id:1},{id:1}]).size` es `2`, no `1` — los objetos se comparan por referencia. Para deduplicar por propiedad, usar Map con clave de propiedad.
> - **Confundir `add` con `push`:** `Set` no tiene `push` ni `append`. El método es siempre `add`.
> - **Esperar que `add` de un duplicado devuelva `false`:** `add` devuelve el Set siempre, tanto si el valor era nuevo como si ya existía. Para saber si era nuevo, comprobar `has` antes.

## Notas relacionadas

- [[03 Set/index|Set — índice]]
- [[02 has, delete, clear, size]]
- [[03 Eliminar Duplicados]]
- [[02 Map/index|Map — índice]]
