---
title: Set — Colección de valores únicos
aliases:
  - Set
  - JavaScript Set
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: Set
tipo: clase
muta: true
asincrono: false
draft: false
---

# Set

> [!definicion]
> `Set` es una colección de valores únicos donde cada valor puede aparecer exactamente una vez. Mantiene el orden de inserción. Usa **SameValueZero** para comparar valores — igual que `===` salvo que `NaN` se considera igual a sí mismo y `+0` es igual a `-0`. No tiene claves: los valores son a la vez clave y dato.

```js
const s = new Set([1, 2, 3, 2, 1]);
console.log(s.size);        // → 3  (duplicados eliminados)
console.log([...s]);        // → [1, 2, 3]  (orden de inserción)

s.add(4).add(4).add(5);     // add es chainable; el segundo 4 se ignora
console.log(s.size);        // → 5
```

## API completa

| Método / Propiedad | Firma | Retorna | Muta |
|--------------------|-------|---------|------|
| Constructor | `new Set()` / `new Set(iterable)` | `Set` | — |
| `add` | `set.add(valor)` | el propio `Set` | sí |
| `has` | `set.has(valor)` | `boolean` | no |
| `delete` | `set.delete(valor)` | `boolean` | sí |
| `clear` | `set.clear()` | `undefined` | sí |
| `size` | `set.size` (propiedad) | `number` | — |
| `keys` | `set.keys()` | `SetIterator` | no |
| `values` | `set.values()` | `SetIterator` | no |
| `entries` | `set.entries()` | `SetIterator` de `[v, v]` | no |
| `forEach` | `set.forEach((v, v, set) => …)` | `undefined` | no |

## Operaciones de conjuntos (ES2025)

ES2025 añade métodos nativos de teoría de conjuntos directamente sobre `Set.prototype`:

| Método | Descripción |
|--------|-------------|
| `union(otroSet)` | Valores en A o en B |
| `intersection(otroSet)` | Valores en A y en B |
| `difference(otroSet)` | Valores en A pero no en B |
| `symmetricDifference(otroSet)` | Valores en A o B pero no en ambos |
| `isSubsetOf(otroSet)` | Todos los de A están en B |
| `isSupersetOf(otroSet)` | Todos los de B están en A |
| `isDisjointFrom(otroSet)` | A y B no comparten elementos |

## Mecanismo interno

`Set` usa una tabla hash con SameValueZero para las búsquedas en O(1). El orden de inserción se mantiene con una lista enlazada interna que recorre los buckets en secuencia — el mismo mecanismo que `Map`. `NaN` se trata como una clave especial que siempre es igual a sí misma; `+0` y `-0` se normalizan a la misma entrada.

## Notas relacionadas

- [[01 Creación y add]]
- [[02 has, delete, clear, size]]
- [[03 Eliminar Duplicados]]
