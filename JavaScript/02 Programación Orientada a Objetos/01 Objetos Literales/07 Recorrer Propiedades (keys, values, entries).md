---
title: Recorrer Propiedades — keys, values, entries, for...in
aliases:
  - Object.keys
  - Object.values
  - Object.entries
  - iterar objeto
  - enumerate properties
tags:
  - javascript
  - api/concepto
  - objetos
objeto: Object
tipo: concepto
muta: false
asincrono: false
draft: false
---

# Recorrer Propiedades (keys, values, entries)

> [!definicion]
> JavaScript ofrece varias APIs para iterar o extraer las propiedades de un objeto. Las diferencias clave son: qué propiedades incluye (propias vs heredadas), si respeta el flag `enumerable`, y si incluye Symbols. `Object.keys`, `Object.values` y `Object.entries` cubren el caso más común: propiedades propias enumerables de tipo string.

```js
const config = {
  host:    "localhost",
  puerto:  3000,
  debug:   true,
};

console.log(Object.keys(config));    // → ["host", "puerto", "debug"]
console.log(Object.values(config));  // → ["localhost", 3000, true]
console.log(Object.entries(config)); // → [["host","localhost"],["puerto",3000],["debug",true]]
```

## Tabla comparativa

| API | Propias | Heredadas | Enumerables | No enumerables | Symbols |
|-----|:-------:|:---------:|:-----------:|:--------------:|:-------:|
| `Object.keys(obj)` | Sí | No | Sí | No | No |
| `Object.values(obj)` | Sí | No | Sí | No | No |
| `Object.entries(obj)` | Sí | No | Sí | No | No |
| `Object.getOwnPropertyNames(obj)` | Sí | No | Sí | Sí | No |
| `Object.getOwnPropertySymbols(obj)` | Sí | No | Sí | Sí | Sí |
| `for...in` | Sí | Sí | Sí | No | No |
| `Reflect.ownKeys(obj)` | Sí | No | Sí | Sí | Sí |

## `Object.keys / values / entries`

Las tres siguen el mismo algoritmo interno `EnumerableOwnProperties`. Devuelven arrays, lo que habilita todos los métodos de array (`map`, `filter`, `reduce`, `find`…).

```js
const precios = { manzana: 1.5, pera: 2.0, uva: 3.5 };

// Filtrar propiedades por valor
const caros = Object.entries(precios)
  .filter(([, precio]) => precio > 2)
  .map(([fruta]) => fruta);
console.log(caros); // → ["pera", "uva"]

// Suma de valores
const total = Object.values(precios).reduce((s, p) => s + p, 0);
console.log(total); // → 7
```

## `Object.getOwnPropertyNames` — incluye no-enumerables

```js
const obj = { visible: 1 };
Object.defineProperty(obj, "oculta", { value: 2, enumerable: false });

console.log(Object.keys(obj));                  // → ["visible"]
console.log(Object.getOwnPropertyNames(obj));   // → ["visible", "oculta"]
```

## `Object.getOwnPropertySymbols` — propiedades Symbol

```js
const ID = Symbol("id");
const usuario = { nombre: "Ana", [ID]: 42 };

console.log(Object.keys(usuario));                    // → ["nombre"]
console.log(Object.getOwnPropertySymbols(usuario));   // → [Symbol(id)]
console.log(usuario[ID]);                             // → 42
```

## `Reflect.ownKeys` — todas las propiedades propias

```js
const ID2 = Symbol("id");
const complejo = { a: 1 };
Object.defineProperty(complejo, "b", { value: 2, enumerable: false });
complejo[ID2] = 3;

console.log(Reflect.ownKeys(complejo)); // → ["a", "b", Symbol(id)]
// Equivale a Object.getOwnPropertyNames + Object.getOwnPropertySymbols
```

## `for...in` — incluye cadena de prototipos

`for...in` itera propiedades enumerables propias **y heredadas**. En la práctica casi siempre se filtra con `Object.hasOwn` para evitar iterar sobre propiedades del prototipo:

```js
const base = { heredada: true };
const hijo = Object.create(base);
hijo.propia = "valor";

for (const clave in hijo) {
  console.log(clave); // → "propia", luego "heredada"
}

for (const clave in hijo) {
  if (Object.hasOwn(hijo, clave)) {
    console.log(clave); // → solo "propia"
  }
}
```

En objetos literales planos (heredan de `Object.prototype`) `for...in` sin filtro raramente produce sorpresas porque las propiedades de `Object.prototype` son no-enumerables. Pero si el prototipo tiene propiedades enumerables añadidas manualmente, aparecerán.

## Recetas

### Objeto a Map

```js
const obj = { a: 1, b: 2 };
const mapa = new Map(Object.entries(obj));
mapa.get("a"); // → 1
```

### Map a objeto

```js
const mapa2 = new Map([["x", 10], ["y", 20]]);
const obj2 = Object.fromEntries(mapa2);
// → { x: 10, y: 20 }
```

### Clonar filtrando propiedades

```js
const usuario2 = { nombre: "Ana", password: "secret", edad: 30 };
const seguro = Object.fromEntries(
  Object.entries(usuario2).filter(([k]) => k !== "password")
);
console.log(seguro); // → { nombre: "Ana", edad: 30 }
```

### Transformar valores (mapear objeto)

```js
const precios2 = { manzana: 1.5, pera: 2.0 };
const conIVA = Object.fromEntries(
  Object.entries(precios2).map(([k, v]) => [k, +(v * 1.21).toFixed(2)])
);
console.log(conIVA); // → { manzana: 1.82, pera: 2.42 }
```

### Invertir claves y valores

```js
const codigo = { ES: "España", FR: "Francia", DE: "Alemania" };
const invertido = Object.fromEntries(Object.entries(codigo).map(([k, v]) => [v, k]));
// → { España: "ES", Francia: "FR", Alemania: "DE" }
```

## Cómo funciona por dentro

`Object.keys`, `Object.values` y `Object.entries` implementan el algoritmo `EnumerableOwnProperties` de la especificación ECMAScript. Este algoritmo recorre la lista interna de propiedades del objeto (en el orden en que fueron añadidas, con la excepción de claves numéricas enteras que se ordenan numéricamente primero), filtra aquellas con `enumerable: true` en su descriptor y excluye los Symbols.

`for...in` usa el algoritmo `[[OwnPropertyKeys]]` combinado con el recorrido de la cadena de prototipos a través de `[[GetPrototypeOf]]`, acumulando claves enumerables en cada nivel.

> [!tip] Buenas prácticas
> - Preferir `Object.entries` + `Object.fromEntries` para transformaciones declarativas de objetos.
> - Usar `Reflect.ownKeys` cuando se necesita un inventario completo (auditoría, serialización personalizada).
> - Evitar `for...in` sin `Object.hasOwn` en código de producción; preferir `Object.keys` + `for...of`.

> [!warning] Errores comunes
> - Asumir que `Object.keys` incluye Symbols: no los incluye. Las APIs de Symbol son invisibles para la mayoría de utilidades.
> - Usar `for...in` sobre arrays: itera los índices como strings más cualquier propiedad enumerable añadida al array o a `Array.prototype`. Usar `for...of` para arrays.
> - El orden de iteración de `Object.keys` en objetos con claves numéricas puede sorprender: las claves enteras aparecen primero en orden ascendente, luego el resto en orden de inserción.

## Notas relacionadas

- [[01 Creación y Propiedades]] — `enumerable` en el descriptor controla qué aparece aquí
- [[08 Comprobar y Eliminar (in, hasOwnProperty, delete)]] — comprobar existencia de propiedades
- [[01 Programación Procedimental/06 Bucles/04 for...in.md | for...in]] — detalle completo del bucle
- [[04 Estructuras de Datos/index | Map]] — alternativa cuando se necesita iterar garantizando orden de inserción con cualquier tipo de clave
