---
title: Map — Iteradores (keys, values, entries)
aliases:
  - map.keys
  - map.values
  - map.entries
  - map.forEach
  - iterar Map
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: Map
tipo: metodo
retorna: MapIterator
muta: false
asincrono: false
draft: false
---

# Map — Iteradores: `keys`, `values`, `entries`

> [!definicion]
> `Map` expone tres iteradores que recorren sus entradas en **orden de inserción**: `map.keys()` produce las claves, `map.values()` los valores, y `map.entries()` pares `[clave, valor]`. `entries` es también el iterador por defecto del `Map` (`map[Symbol.iterator] === map.entries`). `map.forEach` itera con un callback en el orden `(valor, clave, mapa)`. Todos los iteradores reflejan el estado actual del mapa en el momento de ser consumidos.

```js
const m = new Map([["a", 1], ["b", 2], ["c", 3]]);

for (const [clave, valor] of m) {
  console.log(`${clave} → ${valor}`);
}
// → a → 1
// → b → 2
// → c → 3
```

## Firmas

```js
map.keys()               // → MapIterator de claves
map.values()             // → MapIterator de valores
map.entries()            // → MapIterator de [clave, valor]
map[Symbol.iterator]()   // equivalente a map.entries()
map.forEach((valor, clave, mapa) => …) // → undefined
```

## `keys()` — iterador de claves

```js
const precios = new Map([
  ["manzana", 1.5],
  ["pera",    2.0],
  ["uva",     3.5],
]);

for (const producto of precios.keys()) {
  console.log(producto);
}
// → manzana
// → pera
// → uva

console.log([...precios.keys()]); // → ["manzana", "pera", "uva"]
```

## `values()` — iterador de valores

```js
const total = [...precios.values()].reduce((acc, p) => acc + p, 0);
console.log(total); // → 7
```

## `entries()` — iterador de pares

`entries` es el iterador por defecto del Map. `for...of` sobre un Map lo usa implícitamente.

```js
// Explícito
for (const [k, v] of precios.entries()) {
  console.log(`${k}: $${v}`);
}

// Implícito (equivalente)
for (const [k, v] of precios) {
  console.log(`${k}: $${v}`);
}

// Verificar identidad
console.log(precios[Symbol.iterator] === precios.entries); // → true
```

## `forEach` — orden `(valor, clave, mapa)`

El orden de los parámetros del callback es `(valor, clave, mapa)` — inverso al intuitivo. Se eligió así para mantener consistencia con `Array.prototype.forEach(elemento, índice, array)`.

```js
const m = new Map([["x", 10], ["y", 20]]);

m.forEach((valor, clave, mapa) => {
  console.log(`${clave} = ${valor}`);
});
// → x = 10
// → y = 20
```

`forEach` no retorna nada (`undefined`). Para transformar, usar `[...map].map(...)`.

## Conversión a Array

Los iteradores son consumibles una sola vez. La conversión a Array los materializa:

```js
const m = new Map([["a", 1], ["b", 2], ["c", 3]]);

const pares    = [...m];               // → [["a",1], ["b",2], ["c",3]]
const claves   = [...m.keys()];        // → ["a", "b", "c"]
const valores  = [...m.values()];      // → [1, 2, 3]
const paresAlt = Array.from(m);        // equivalente a [...m]
```

## Recetas comunes

### Filtrar un Map por valor

```js
const precios = new Map([
  ["manzana", 1.5],
  ["pera",    2.0],
  ["uva",     3.5],
]);

const caros = new Map(
  [...precios].filter(([k, v]) => v > 1.8)
);
console.log([...caros.keys()]); // → ["pera", "uva"]
```

### Transformar valores

```js
const conIVA = new Map(
  [...precios].map(([k, v]) => [k, +(v * 1.21).toFixed(2)])
);
console.log(conIVA.get("manzana")); // → 1.82
```

### Invertir clave ↔ valor

```js
const original = new Map([["a", 1], ["b", 2], ["c", 3]]);
const invertido = new Map([...original].map(([k, v]) => [v, k]));
console.log(invertido.get(2)); // → "b"
```

### Combinar dos Maps

```js
const m1 = new Map([["a", 1], ["b", 2]]);
const m2 = new Map([["b", 99], ["c", 3]]);

// m2 sobreescribe claves de m1 en caso de colisión
const combinado = new Map([...m1, ...m2]);
console.log(combinado.get("b")); // → 99
```

### Convertir Map a objeto plano

```js
const config = new Map([["host", "localhost"], ["port", 3000]]);
const obj = Object.fromEntries(config);
console.log(obj); // → { host: "localhost", port: 3000 }
```

## Cómo funciona por dentro

> [!info] Iteradores lazy
> Los métodos `keys()`, `values()` y `entries()` retornan objetos iterador que implementan el protocolo `{ next() }`. Son **lazy**: no materializan los datos hasta que se consume cada elemento. El iterador mantiene un cursor interno que avanza por la lista enlazada del `Map` en orden de inserción. Si el mapa se modifica durante la iteración (`set` o `delete`), el comportamiento es definido por la especificación: las nuevas entradas añadidas **después** del cursor actual se visitarán; las entradas eliminadas antes de ser visitadas se saltan; las ya visitadas no se revisitan.

> [!tip] Buenas prácticas
> - Preferir `for...of` con destructuring `[k, v]` sobre `forEach` para mayor legibilidad y posibilidad de usar `break` y `continue`.
> - Usar `[...map]` solo cuando se necesite el array completo; si basta iterar una vez, `for...of` es más eficiente en memoria.
> - Para transformaciones (filter, map), el patrón `new Map([...m].filter/map(...))` es idiomático y claro.

> [!warning] Errores comunes
> - **Orden invertido en `forEach`:** `(valor, clave)` no `(clave, valor)`. Es el error más frecuente al migrar desde iterar objetos con `Object.entries`.
> - **Iterador consumido:** un `MapIterator` solo puede recorrerse una vez. Después de agotarlo, `next()` retorna `{ value: undefined, done: true }`. Convertir a array primero si se necesita reutilizar.
> - **Modificar el mapa durante iteración `for...of`:** añadir entradas durante la iteración puede producir bucles infinitos si las nuevas claves se insertan al final y el cursor aún no las alcanzó.

## Notas relacionadas

- [[02 Map/index|Map — índice]]
- [[01 Creación y set/get|Creación, set y get]]
- [[02 has, delete, clear, size]]
- [[04 Diferencias con Objetos]]
