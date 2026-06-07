---
title: Map — Colección clave-valor con claves de cualquier tipo
aliases:
  - Map
  - JavaScript Map
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: Map
tipo: clase
muta: true
asincrono: false
draft: false
---

# Map

> [!definicion]
> `Map` es una colección de pares clave-valor donde **las claves pueden ser de cualquier tipo** — objetos, funciones, primitivos, `NaN` — y el orden de inserción se preserva siempre. A diferencia de un objeto literal, `Map` no tiene prototipo heredado que contamine las claves, soporta `size` en O(1) y está optimizado para operaciones dinámicas de inserción y eliminación frecuentes.

```js
const m = new Map();
m.set("nombre", "Ana");
m.set(42, "el número");
m.set({id: 1}, "objeto como clave");

console.log(m.size); // → 3
```

## API completa

| Método / Propiedad | Firma | Retorna | Muta |
|--------------------|-------|---------|------|
| Constructor | `new Map()` / `new Map(iterable)` | `Map` | — |
| `set` | `map.set(clave, valor)` | el propio `Map` | sí |
| `get` | `map.get(clave)` | valor o `undefined` | no |
| `has` | `map.has(clave)` | `boolean` | no |
| `delete` | `map.delete(clave)` | `boolean` | sí |
| `clear` | `map.clear()` | `undefined` | sí |
| `size` | `map.size` (propiedad) | `number` | — |
| `keys` | `map.keys()` | `MapIterator` | no |
| `values` | `map.values()` | `MapIterator` | no |
| `entries` | `map.entries()` | `MapIterator` | no |
| `forEach` | `map.forEach((v, k, m) => …)` | `undefined` | no |

## Mecanismo interno

`Map` usa una tabla hash con comparación **SameValueZero** (igual que `===`, salvo que `NaN === NaN` es `true`). Las claves objeto se comparan por referencia de identidad, no por contenido. El orden de inserción se mantiene mediante una lista enlazada interna que recorre los buckets en secuencia de inserción.

## Notas relacionadas

- [[01 Creación y set/get]]
- [[02 has, delete, clear, size]]
- [[03 Iteradores (keys, values, entries)]]
- [[04 Diferencias con Objetos]]
