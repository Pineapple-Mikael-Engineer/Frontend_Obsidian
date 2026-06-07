---
title: Map — Creación, set y get
aliases:
  - new Map
  - map.set
  - map.get
  - Map constructor
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: Map
tipo: metodo
retorna: Map
muta: true
asincrono: false
draft: false
---

# Map — Creación, `set` y `get`

> [!definicion]
> `new Map()` crea una colección clave-valor vacía o inicializada desde un iterable de pares `[clave, valor]`. `map.set(clave, valor)` añade o actualiza una entrada y retorna el propio `Map` (encadenable). `map.get(clave)` retorna el valor asociado o `undefined` si la clave no existe. Las claves se comparan con **SameValueZero** — idéntico a `===` excepto que `NaN === NaN` es `true`.

```js
// Mapa vacío
const m = new Map();

// set encadenable
m.set("a", 1).set("b", 2).set("c", 3);

// get
console.log(m.get("a")); // → 1
console.log(m.get("z")); // → undefined
```

## Firmas

```js
new Map()
new Map(iterable)   // iterable de pares [clave, valor]

map.set(clave, valor)  // → Map  (muta y retorna el propio mapa)
map.get(clave)         // → valor | undefined
```

## Formas de construcción

### Mapa vacío

```js
const inventario = new Map();
inventario.set("manzana", 40);
inventario.set("pera", 15);
```

### Desde array de pares

El constructor acepta cualquier iterable cuyos elementos sean pares `[clave, valor]`:

```js
const colores = new Map([
  ["rojo",   "#e74c3c"],
  ["verde",  "#2ecc71"],
  ["azul",   "#3498db"],
]);

console.log(colores.get("verde")); // → "#2ecc71"
```

### Desde `Object.entries` — convertir objeto a Map

```js
const config = { host: "localhost", puerto: 3000, debug: true };
const configMap = new Map(Object.entries(config));

console.log(configMap.get("puerto")); // → 3000
```

### Clonar un Map existente

```js
const original = new Map([["x", 10], ["y", 20]]);
const copia = new Map(original); // itera sobre original (es iterable de pares)
```

## Claves de cualquier tipo

La ventaja fundamental de `Map` sobre los objetos es aceptar cualquier valor como clave.

### Objetos como clave

```js
const nodo1 = { id: 1, label: "A" };
const nodo2 = { id: 2, label: "B" };
const metadata = new Map();

metadata.set(nodo1, { visitado: false, coste: 0 });
metadata.set(nodo2, { visitado: false, coste: Infinity });

console.log(metadata.get(nodo1).coste); // → 0
// metadata.get({ id: 1, label: "A" }) → undefined  (distinta referencia)
```

### Funciones como clave

```js
function handler() {}
const cache = new Map();
cache.set(handler, { ultimaLlamada: Date.now() });
console.log(cache.get(handler)); // → { ultimaLlamada: ... }
```

### `NaN` como clave

SameValueZero hace que `NaN` se identifique a sí mismo como clave:

```js
const m = new Map();
m.set(NaN, "valor especial");
console.log(m.get(NaN));        // → "valor especial"
console.log(m.get(NaN) !== undefined); // → true
```

## Recetas comunes

### Caché de resultados con objeto como clave (memoización por referencia)

```js
const cache = new Map();

function calcularPeso(nodo) {
  if (cache.has(nodo)) return cache.get(nodo);
  const resultado = nodo.hijos.reduce((acc, h) => acc + h.peso, 0);
  cache.set(nodo, resultado);
  return resultado;
}
```

La clave es la referencia al objeto — si el mismo objeto se reutiliza, el caché acierta.

### Contar ocurrencias de elementos

```js
const palabras = ["hola", "mundo", "hola", "js", "hola", "mundo"];
const frecuencia = new Map();

for (const p of palabras) {
  frecuencia.set(p, (frecuencia.get(p) ?? 0) + 1);
}

console.log(frecuencia.get("hola"));  // → 3
console.log(frecuencia.get("mundo")); // → 2
```

El patrón `(map.get(k) ?? 0) + 1` evita el `NaN` que produciría `undefined + 1`.

### Agrupar elementos

```js
const productos = [
  { nombre: "A", categoria: "fruta" },
  { nombre: "B", categoria: "verdura" },
  { nombre: "C", categoria: "fruta" },
];

const grupos = new Map();
for (const p of productos) {
  if (!grupos.has(p.categoria)) grupos.set(p.categoria, []);
  grupos.get(p.categoria).push(p.nombre);
}

console.log(grupos.get("fruta")); // → ["A", "C"]
```

## Cómo funciona por dentro

> [!info] SameValueZero y tabla hash
> `Map` usa internamente una tabla hash. La comparación de claves sigue el algoritmo **SameValueZero**: equivalente a `===` para todos los tipos excepto `NaN`, que se iguala a sí mismo. Las claves de tipo objeto se almacenan por **referencia de identidad** — dos objetos con el mismo contenido son claves distintas. El orden de inserción se preserva mediante una lista enlazada que recorre los buckets en secuencia de inserción, por lo que la iteración es siempre predecible.

## `set` es encadenable

`map.set(k, v)` retorna el propio mapa, permitiendo cadenas fluidas:

```js
const m = new Map()
  .set("host", "localhost")
  .set("port", 5432)
  .set("db",   "produccion");
```

> [!tip] Buenas prácticas
> - Usar `new Map(Object.entries(obj))` para convertir objetos de configuración a Map cuando se necesite iteración o `size`.
> - Preferir `map.get(k) ?? valorDefecto` sobre `map.get(k) || valorDefecto` para evitar falsos negativos con `0`, `""` o `false`.
> - Cuando la clave es un objeto, guardar siempre la misma referencia; no reconstruir el objeto en cada acceso.

> [!warning] Errores comunes
> - **Clave objeto por valor distinto:** `m.get({ id: 1 })` siempre retorna `undefined` si la clave insertada fue un objeto diferente (aunque con igual contenido). La igualdad es por referencia.
> - **`new Map(objeto)`** no funciona: el constructor espera un iterable de pares. Para un objeto plano usar `new Map(Object.entries(objeto))`.
> - **Mutar el objeto-clave** después de insertarlo no cambia su identidad en el mapa, pero puede producir confusión lógica si el `get` depende de propiedades del objeto.

## Notas relacionadas

- [[02 Map/index|Map — índice]]
- [[02 has, delete, clear, size]]
- [[03 Iteradores (keys, values, entries)]]
- [[04 Diferencias con Objetos]]
