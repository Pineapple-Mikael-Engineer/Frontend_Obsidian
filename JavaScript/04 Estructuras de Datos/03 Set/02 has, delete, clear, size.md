---
title: Set — has, delete, clear, size
aliases:
  - set.has
  - set.delete
  - set.clear
  - set.size
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: Set
tipo: metodo
retorna: boolean
muta: true
asincrono: false
draft: false
order: 2
---

# Set — `has`, `delete`, `clear`, `size`

> [!definicion]
> Cuatro operaciones de consulta y eliminación sobre un `Set`: `has(valor)` comprueba si el valor está en O(1); `delete(valor)` lo elimina y reporta si existía; `clear()` vacía el conjunto completamente; `size` es una propiedad getter que devuelve el número de valores únicos en O(1), siempre sincronizada.

```js
const s = new Set(["a", "b", "c", "d"]);

console.log(s.has("b"));   // → true
console.log(s.size);       // → 4

s.delete("b");
console.log(s.has("b"));   // → false
console.log(s.size);       // → 3

s.clear();
console.log(s.size);       // → 0
```

## Firmas

```js
set.has(valor)    // → boolean
set.delete(valor) // → boolean  (true si existía, false si no)
set.clear()       // → undefined
set.size          // → number   (propiedad getter, no método)
```

## `has` — comprobar existencia en O(1)

`has` usa SameValueZero — la misma comparación que `add` — para buscar el valor en la tabla hash interna. La búsqueda es O(1) independientemente del tamaño del Set.

```js
const permisos = new Set(["leer", "escribir", "admin"]);

if (permisos.has("admin")) {
  console.log("Acceso total"); // → "Acceso total"
}

// NaN se encuentra correctamente
const s = new Set([NaN, 1, 2]);
console.log(s.has(NaN)); // → true  (a diferencia de Array.includes con NaN... espera, includes sí lo maneja)

// Objetos: busca por referencia
const obj = { id: 1 };
const s2 = new Set([obj]);
console.log(s2.has(obj));       // → true
console.log(s2.has({ id: 1 })); // → false  (objeto distinto)
```

## `delete` — eliminar un valor

`delete` elimina el valor y retorna `true` si existía, `false` si no. No lanza error con valores ausentes. El valor de retorno permite saber si el Set cambió.

```js
const activos = new Set([101, 102, 103]);

console.log(activos.delete(102)); // → true   (existía)
console.log(activos.delete(999)); // → false  (no existía)
console.log([...activos]);        // → [101, 103]
```

### Eliminar durante iteración

Es seguro eliminar la entrada actual durante un `forEach` o `for...of`. Las entradas añadidas durante la iteración sí se visitan; las eliminadas antes de ser visitadas no se visitan.

```js
const s = new Set([1, 2, 3, 4, 5]);

for (const v of s) {
  if (v % 2 === 0) s.delete(v); // elimina 2 y 4 de forma segura
}

console.log([...s]); // → [1, 3, 5]
```

## `clear` — vaciar el conjunto

`clear` elimina todas las entradas en una operación. Es más eficiente que iterar y llamar `delete` por cada valor porque el motor puede liberar la estructura interna directamente.

```js
const cache = new Set();
cache.add("hash1");
cache.add("hash2");
cache.add("hash3");

console.log(cache.size); // → 3
cache.clear();
console.log(cache.size); // → 0
```

## `size` — tamaño en O(1)

`size` es una propiedad getter (no un método — no se llama como función). Siempre refleja el estado actual: se actualiza tras cada `add`, `delete` y `clear`.

```js
const s = new Set();
console.log(s.size); // → 0

s.add(1); s.add(2); s.add(3);
console.log(s.size); // → 3

s.add(2);            // duplicado — ignorado
console.log(s.size); // → 3  (no cambia)

s.delete(1);
console.log(s.size); // → 2
```

## Operaciones de conjuntos pre-ES2025

Antes de que ES2025 añadiera métodos nativos, las operaciones clásicas se implementan combinando `has`, spread y `filter`:

```js
const a = new Set([1, 2, 3, 4]);
const b = new Set([3, 4, 5, 6]);

// Unión: todo lo que está en A o en B
const union = new Set([...a, ...b]);
console.log([...union]); // → [1, 2, 3, 4, 5, 6]

// Intersección: solo lo que está en ambos
const interseccion = new Set([...a].filter(x => b.has(x)));
console.log([...interseccion]); // → [3, 4]

// Diferencia A − B: en A pero no en B
const diferencia = new Set([...a].filter(x => !b.has(x)));
console.log([...diferencia]); // → [1, 2]

// Diferencia simétrica: en A o en B pero no en ambos
const simetrica = new Set([
  ...[...a].filter(x => !b.has(x)),
  ...[...b].filter(x => !a.has(x)),
]);
console.log([...simetrica]); // → [1, 2, 5, 6]
```

## Operaciones de conjuntos nativos (ES2025)

```js
const a = new Set([1, 2, 3, 4]);
const b = new Set([3, 4, 5, 6]);

console.log([...a.union(b)]);              // → [1, 2, 3, 4, 5, 6]
console.log([...a.intersection(b)]);       // → [3, 4]
console.log([...a.difference(b)]);         // → [1, 2]
console.log([...a.symmetricDifference(b)]); // → [1, 2, 5, 6]
console.log(a.isSubsetOf(new Set([1,2,3,4,5]))); // → true
console.log(a.isDisjointFrom(new Set([7,8])));    // → true
```

## Recetas comunes

### Filtrar una lista contra un conjunto de exclusión

```js
const bloqueados = new Set(["spam@x.com", "bot@y.com"]);
const solicitudes = ["user@a.com", "spam@x.com", "dev@b.com", "bot@y.com"];

const validas = solicitudes.filter(email => !bloqueados.has(email));
console.log(validas); // → ["user@a.com", "dev@b.com"]
```

### Contar valores únicos

```js
function contarUnicos(arr) {
  return new Set(arr).size;
}
console.log(contarUnicos([1, 2, 2, 3, 3, 3])); // → 3
```

### Comprobar si dos arrays tienen los mismos elementos (sin orden)

```js
function mismosElementos(a, b) {
  const sa = new Set(a);
  const sb = new Set(b);
  if (sa.size !== sb.size) return false;
  return [...sa].every(v => sb.has(v));
}
console.log(mismosElementos([1,2,3], [3,1,2])); // → true
console.log(mismosElementos([1,2,3], [1,2,4])); // → false
```

## Cómo funciona por dentro

`has` y `delete` calculan el hash del valor con SameValueZero y lo buscan directamente en el bucket correspondiente — O(1) amortizado. `size` es un contador interno que el motor mantiene sincronizado: se incrementa en `add` solo cuando el valor es nuevo, se decrementa en `delete` solo cuando el valor existía, y se pone a cero en `clear`. No hay recuento diferido ni recorrido interno.

> [!tip] Buenas prácticas
> - Usar `set.has(v)` en lugar de `[...set].includes(v)` — el primero es O(1), el segundo es O(n).
> - Aprovechar el valor de retorno de `delete` cuando el flujo necesita distinguir "eliminado" de "no existía".
> - Para limpiar un Set compartido entre múltiples referencias, usar `clear()` en lugar de reasignar la variable — reasignar no afecta a quien tiene la referencia anterior.

> [!warning] Errores comunes
> - **Llamar `size` como función:** `set.size()` lanza `TypeError` — es una propiedad getter, no un método.
> - **Usar `has` con objetos literales:** `set.has({id:1})` siempre devuelve `false` a menos que ese objeto exacto (misma referencia) esté en el Set.
> - **Confiar en `delete` sin comprobar el retorno:** cuando importa saber si el valor existía, usar el booleano devuelto, no solo llamar `delete`.

## Notas relacionadas

- [[03 Set/index|Set — índice]]
- [[01 Creación y add]]
- [[03 Eliminar Duplicados]]
- [[02 Map/02 has, delete, clear, size|Map — has, delete, clear, size]]
