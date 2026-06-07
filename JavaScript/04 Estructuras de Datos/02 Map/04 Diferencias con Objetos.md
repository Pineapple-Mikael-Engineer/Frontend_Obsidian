---
title: Map vs Object — Diferencias y cuándo usar cada uno
aliases:
  - Map vs Object
  - cuándo usar Map
  - diferencias Map Object
tags:
  - javascript
  - api/concepto
  - estructuras-datos
objeto: Map
tipo: concepto
muta: false
asincrono: false
draft: false
---

# Map vs Object — Diferencias y cuándo usar cada uno

> [!definicion]
> `Map` y `Object` son las dos estructuras clave-valor principales de JavaScript. Se diferencian en tipo de clave aceptada, orden garantizado, obtención de tamaño, rendimiento en operaciones dinámicas, serialización JSON y ausencia de contaminación prototípica. La elección depende del patrón de uso, no de preferencia personal.

```js
// Object — claves solo string/Symbol, acceso estático
const config = { host: "localhost", port: 3000 };
console.log(config.host);            // → "localhost"

// Map — claves de cualquier tipo, acceso dinámico
const configMap = new Map(Object.entries(config));
console.log(configMap.get("host"));  // → "localhost"
console.log(configMap.size);         // → 2  (O(1))
```

## Tabla comparativa

| Característica | Map | Object |
|----------------|-----|--------|
| Tipo de clave | Cualquier valor (objetos, funciones, NaN, primitivos) | Solo `string` o `Symbol` |
| Orden de iteración | Orden de inserción garantizado (ES2015+) | Orden de inserción para strings no-enteros; enteros primero (ES2015+) |
| Tamaño | `map.size` — O(1), siempre actualizado | `Object.keys(obj).length` — O(n) |
| Iteración directa | `for...of map`, `map.keys()`, `map.values()` | `for...in` (incluye heredadas), `Object.keys/values/entries` |
| Serialización JSON | `JSON.stringify(map)` produce `"{}"` | `JSON.stringify(obj)` serializa correctamente |
| Contaminación prototípica | No — Map no hereda propiedades en las claves | Sí — claves como `"toString"`, `"constructor"` colisionan con `Object.prototype` |
| Rendimiento (inserciones/eliminaciones frecuentes) | Optimizado para tabla hash dinámica | Optimizado para propiedades estáticas (hidden class en V8) |
| Rendimiento (acceso a propiedad única) | Más lento que propiedad de objeto | Más rápido para propiedades estáticas conocidas en compilación |

## Cuándo usar Map

- Las claves no son strings ni Symbols: objetos, funciones, valores primitivos mixtos.
- Se necesita `size` eficiente sin recorrido.
- El conjunto de claves crece y encoge dinámicamente con frecuencia.
- Se itera el mapa completo con frecuencia — Map es más rápido que Object en iteración cuando hay muchas entradas.
- Se necesita garantía de orden de inserción sin excepciones (los enteros como claves de objeto tienen orden especial).

```js
// Caso típico: metadatos asociados a objetos sin modificarlos
const metadatos = new Map();
const usuario = { id: 42, nombre: "Ana" };
metadatos.set(usuario, { ultimoAcceso: new Date(), rol: "admin" });
```

## Cuándo usar Object

- Estructura de datos con forma conocida en tiempo de escritura (DTO, configuración, respuesta de API).
- Se necesita serialización JSON directa sin transformación.
- Se accede a propiedades por nombre estático en código: `obj.nombre`, `obj.host` — más legible que `map.get("nombre")`.
- Se pasa como argumento a APIs que esperan un objeto plano.

```js
// Caso típico: DTO de respuesta de API
const usuario = {
  id: 1,
  nombre: "Ana",
  email: "ana@ejemplo.com",
};
const json = JSON.stringify(usuario); // funciona directamente
```

## Serialización JSON

`JSON.stringify` no sabe serializar un `Map` — lo trata como objeto vacío:

```js
const m = new Map([["a", 1], ["b", 2]]);
console.log(JSON.stringify(m)); // → "{}"
```

Para serializar un Map, convertirlo primero a objeto plano o a array de pares:

```js
// A objeto plano (claves deben ser strings)
const obj = Object.fromEntries(m);
console.log(JSON.stringify(obj)); // → '{"a":1,"b":2}'

// A array de pares (preserva claves no-string)
const pares = JSON.stringify([...m]);
console.log(pares); // → '[["a",1],["b",2]]'

// Restaurar
const restaurado = new Map(JSON.parse(pares));
```

## Contaminación prototípica

Un objeto literal hereda de `Object.prototype`, por lo que las claves `"toString"`, `"valueOf"`, `"constructor"`, `"hasOwnProperty"` ya existen como propiedades heredadas:

```js
const obj = {};
console.log("toString" in obj);      // → true  (heredada)
console.log("constructor" in obj);   // → true  (heredada)

// Map no tiene este problema
const m = new Map();
console.log(m.has("toString"));      // → false
console.log(m.has("constructor"));   // → false
```

Un objeto creado con `Object.create(null)` elimina la herencia, pero pierde métodos útiles como `toString`.

## Rendimiento

> [!info] Modelo interno en V8
> Los objetos JS usan una representación interna llamada **hidden class** (o *shape/map* en V8): cuando se añaden propiedades siempre en el mismo orden y con los mismos tipos, V8 puede compilar el acceso a velocidad cercana a C++. Esta optimización se degrada si las propiedades cambian dinámicamente (diferentes propiedades en distintos objetos del mismo "tipo" o adición/eliminación frecuente). `Map`, en cambio, usa una tabla hash diseñada explícitamente para operaciones dinámicas — inserción y eliminación son O(1) amortizado sin degradación por cambios de shape. Para objetos con propiedades fijas, el objeto es más rápido en acceso; para colecciones con claves variables, Map escala mejor.

```js
// Benchmark conceptual — no ejecutar como benchmark real
const N = 100_000;

// Object: inserción frecuente degrada la hidden class
const obj = {};
for (let i = 0; i < N; i++) obj[`key${i}`] = i;

// Map: tabla hash diseñada para esto
const map = new Map();
for (let i = 0; i < N; i++) map.set(`key${i}`, i);
// Map es más eficiente para este patrón
```

> [!tip] Buenas prácticas
> - Usar `Map` como estructura de datos de propósito general cuando las claves son dinámicas o no son strings.
> - Usar `Object` para modelos de datos con forma conocida (interfaces, configuraciones, DTOs).
> - Convertir Map a objeto para serialización: `Object.fromEntries(map)`.
> - Convertir objeto a Map para procesamiento dinámico: `new Map(Object.entries(obj))`.

> [!warning] Errores comunes
> - **Usar `JSON.stringify` directamente sobre un Map** y confundirse al obtener `"{}"`. Siempre pasar por `Object.fromEntries` o `[...map]` antes de serializar.
> - **Usar `for...in` para iterar un Map** no funciona — `for...in` itera propiedades enumerables del objeto, no entradas del Map.
> - **Asumir que un Object no hereda propiedades:** las claves `"toString"`, `"hasOwnProperty"` etc. existen en el prototipo. Para un registro de claves arbitrarias con un objeto, usar `Object.create(null)` o preferir `Map`.

## Notas relacionadas

- [[02 Map/index|Map — índice]]
- [[01 Creación y set/get|Creación, set y get]]
- [[02 has, delete, clear, size]]
- [[03 Iteradores (keys, values, entries)]]
