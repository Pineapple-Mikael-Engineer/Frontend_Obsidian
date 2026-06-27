---
title: Array — Creación
aliases:
  - crear array
  - Array.from
  - Array.of
  - new Array
tags:
  - javascript
  - api/concepto
  - arrays
objeto: Array
tipo: concepto
muta: false
asincrono: false
draft: false
order: 1
---

# Array — Creación

> [!definicion]
> Existen cinco formas principales de crear un array en JavaScript: literal `[]`, `new Array(n)`, `Array.of(...elems)`, `Array.from(iterable)` y `Array(n).fill(valor)`. Cada una tiene semántica distinta; confundirlas genera arrays con comportamientos inesperados.

## Formas de creación

### Literal `[]`

La forma idiomática y sin ambigüedad. Siempre preferirla sobre `new Array` cuando los elementos se conocen en tiempo de escritura.

```js
const vacio = [];
const nums  = [1, 2, 3];
const mixto = [1, "dos", true, null];
```

### `new Array(n)` — array vacío con longitud fijada

Con un único argumento numérico, `new Array(n)` crea un array de longitud `n` **con huecos** (slots no inicializados), no con `undefined`. Los huecos no son propiedades reales del objeto.

```js
const a = new Array(3);
console.log(a.length);      // → 3
console.log(a[0]);          // → undefined (lectura de propiedad inexistente)
console.log(0 in a);        // → false  ← el slot NO existe
```

> [!warning] `new Array(n).map(...)` no funciona
> Los métodos iterativos (`map`, `filter`, `forEach`) saltan huecos porque los slots no existen como propiedades enumerables. `new Array(3).map(() => 0)` devuelve `[empty × 3]`, no `[0, 0, 0]`. Para rellenar, usar `.fill()` primero o `Array.from`.

### `new Array(elem1, elem2, ...)` — con múltiples argumentos

Con dos o más argumentos, `new Array` crea un array con esos elementos (sin ambigüedad de longitud).

```js
const b = new Array(1, 2, 3);
console.log(b); // → [1, 2, 3]

// Trampa: new Array(3) ≠ new Array(3, ?)
// new Array(3)    → [ , , ]      longitud 3, sin elementos
// new Array(3, 4) → [3, 4]       dos elementos
```

### `Array.of(...elems)` — sin la ambigüedad de `new Array`

`Array.of` siempre crea un array con los argumentos como elementos, independientemente de su cantidad o tipo. Creada precisamente para eliminar la trampa del argumento único entero.

```js
Array.of(3);       // → [3]        ← un elemento, no longitud
Array.of(1, 2, 3); // → [1, 2, 3]
```

### `Array.from(iterable, mapFn?)` — desde iterables y array-likes

Acepta cualquier iterable (`string`, `Set`, `Map`, `NodeList`, generadores) o array-like (objeto con `length` y propiedades numéricas como `arguments`). El segundo parámetro opcional es una función de mapeo aplicada a cada elemento durante la conversión.

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `arrayLike` | iterable o array-like | Fuente de elementos |
| `mapFn` | `(elem, index) => valor` (opcional) | Transforma cada elemento al crear |
| `thisArg` | any (opcional) | Contexto de `mapFn` |

```js
// Desde string
Array.from("hola");              // → ["h", "o", "l", "a"]

// Desde Set
Array.from(new Set([1, 2, 2, 3])); // → [1, 2, 3]

// Desde array-like
Array.from({ length: 3 }, (_, i) => i * 2); // → [0, 2, 4]

// Desde generador
function* rango(n) { for (let i = 0; i < n; i++) yield i; }
Array.from(rango(4)); // → [0, 1, 2, 3]
```

### `Array(n).fill(valor)` — array de n copias de un valor

La forma idiomática para crear un array de longitud fija con un valor inicial. `fill` inicializa los huecos, convirtiendo el sparse array en uno con slots reales.

```js
Array(5).fill(0);    // → [0, 0, 0, 0, 0]
Array(3).fill(null); // → [null, null, null]
```

> [!warning] Trampa con objetos en `fill`
> `Array(3).fill({})` hace que las tres posiciones apunten **al mismo objeto**. Mutar uno muta todos. Para objetos independientes, usar `Array.from`:
> ```js
> // MAL — tres referencias al mismo objeto
> const mal = Array(3).fill({});
> mal[0].x = 1;
> console.log(mal[1].x); // → 1 (¡compartido!)
>
> // BIEN — tres objetos distintos
> const bien = Array.from({ length: 3 }, () => ({}));
> bien[0].x = 1;
> console.log(bien[1].x); // → undefined
> ```

## Sparse arrays (arrays con huecos)

Un array sparse contiene huecos: posiciones con índice válido pero sin propiedad real en el objeto.

```js
const sparse = [1, , 3];  // índice 1 es un hueco
console.log(sparse.length); // → 3
console.log(1 in sparse);   // → false
console.log(sparse[1]);     // → undefined (lectura de prop inexistente)

// Los métodos los tratan de forma inconsistente:
sparse.map(x => x * 2);    // → [2, empty, 6]  — map salta huecos
sparse.join("-");           // → "1--3"          — join los trata como ""
[...sparse];                // → [1, undefined, 3] — spread los materializa
```

> [!tip] Buenas prácticas
> - Usar `[]` para la mayoría de los casos.
> - Usar `Array.from({ length: n }, mapFn)` para arrays inicializados con lógica.
> - Usar `Array(n).fill(primitivo)` para arrays de valor único.
> - Evitar `new Array(n)` a secas salvo preallocación explícita con `fill` inmediato.
> - Nunca crear sparse arrays intencionalmente con comas sueltas `[1,,3]` — confunden a los lectores y a los métodos iterativos.

## Cómo funciona por dentro

Los arrays en JS son objetos cuyas claves son **strings** que representan enteros no negativos (el motor los convierte: `arr[0]` accede a la propiedad `"0"`). La propiedad `length` se actualiza automáticamente cuando se asigna a un índice numérico igual o mayor a `length` actual.

V8 mantiene varios "tipos de elementos" internos para optimizar el almacenamiento: `PACKED_SMI_ELEMENTS` (enteros), `PACKED_DOUBLE_ELEMENTS` (doubles), `PACKED_ELEMENTS` (cualquier objeto), y sus variantes `HOLEY_*` para arrays con huecos. Una vez que el tipo se degrada (de SMI a double, de packed a holey), no sube de nuevo — es un motivo para evitar huecos.

## Notas relacionadas

- [[02 Acceso por Índice y length]]
- [[12 fill y copyWithin]]
- [[14 Arrays Multidimensionales]]
- [[index|Arrays]]
