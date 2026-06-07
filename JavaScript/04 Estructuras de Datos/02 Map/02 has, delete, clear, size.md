---
title: Map — has, delete, clear, size
aliases:
  - map.has
  - map.delete
  - map.clear
  - map.size
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: Map
tipo: metodo
retorna: boolean
muta: true
asincrono: false
draft: false
---

# Map — `has`, `delete`, `clear`, `size`

> [!definicion]
> Cuatro operaciones de consulta y eliminación sobre un `Map`: `has(clave)` comprueba existencia en O(1); `delete(clave)` elimina una entrada y reporta si existía; `clear()` vacía el mapa completamente; `size` es una propiedad (no método) que devuelve el número de entradas en O(1), siempre sincronizada con el estado interno.

```js
const m = new Map([["a", 1], ["b", 2], ["c", 3]]);

console.log(m.has("b"));    // → true
console.log(m.size);        // → 3

m.delete("b");
console.log(m.has("b"));    // → false
console.log(m.size);        // → 2

m.clear();
console.log(m.size);        // → 0
```

## Firmas

```js
map.has(clave)    // → boolean
map.delete(clave) // → boolean  (true si existía, false si no)
map.clear()       // → undefined
map.size          // → number   (propiedad, no llamada)
```

## `has` — comprobar existencia

`has` retorna `true` si la clave existe en el mapa usando SameValueZero, `false` en caso contrario. No distingue entre "clave ausente" y "clave con valor `undefined`" — para eso es necesario comparar `map.get(k) !== undefined` junto con `map.has(k)`.

```js
const m = new Map([["activo", undefined], ["nombre", "Ana"]]);

console.log(m.has("activo"));  // → true   (la clave existe aunque el valor sea undefined)
console.log(m.has("edad"));    // → false

// Distinguir clave-ausente de clave-con-undefined:
if (m.has("activo") && m.get("activo") === undefined) {
  console.log("Clave presente con valor explícitamente undefined");
}
```

## `delete` — eliminar una entrada

`delete` elimina la entrada correspondiente a la clave y retorna `true` si la entrada existía o `false` si no. No lanza error con claves inexistentes.

```js
const sesiones = new Map();
sesiones.set("user:42", { token: "abc", expira: Date.now() + 3600000 });

const eliminada = sesiones.delete("user:42");
console.log(eliminada);              // → true
console.log(sesiones.has("user:42")); // → false

const inexistente = sesiones.delete("user:99");
console.log(inexistente);            // → false  (no lanza error)
```

## `clear` — vaciar el mapa

`clear` elimina todas las entradas en una sola operación. Más eficiente que iterar y llamar `delete` por cada clave.

```js
const cache = new Map();
cache.set("req:1", { datos: [1, 2, 3] });
cache.set("req:2", { datos: [4, 5, 6] });

cache.clear();
console.log(cache.size); // → 0
```

## `size` — tamaño en O(1)

`size` es una propiedad getter que retorna el número exacto de entradas. Se actualiza automáticamente tras cada `set`, `delete` y `clear`. No recorre el mapa — es O(1).

```js
const m = new Map([["x", 1], ["y", 2]]);
console.log(m.size); // → 2

m.set("z", 3);
console.log(m.size); // → 3

m.delete("x");
console.log(m.size); // → 2
```

**Contraste con Object:** obtener el tamaño de un objeto requiere `Object.keys(obj).length`, que es O(n) porque itera todas las claves enumerables. `Map.size` es siempre O(1).

```js
const obj = { a: 1, b: 2, c: 3 };
console.log(Object.keys(obj).length); // → 3  (O(n))

const m = new Map([["a", 1], ["b", 2], ["c", 3]]);
console.log(m.size); // → 3  (O(1))
```

## Recetas comunes

### Patrón set-if-absent (inicialización lazy)

```js
function getOrCreate(map, clave, fabrica) {
  if (!map.has(clave)) map.set(clave, fabrica());
  return map.get(clave);
}

const grupos = new Map();
getOrCreate(grupos, "fruta", () => []).push("manzana");
getOrCreate(grupos, "fruta", () => []).push("pera");
getOrCreate(grupos, "verdura", () => []).push("zanahoria");

console.log(grupos.get("fruta")); // → ["manzana", "pera"]
```

### Verificar y eliminar con reporte

```js
function revocarSesion(sesiones, userId) {
  if (!sesiones.has(userId)) {
    console.warn(`Sesión no encontrada: ${userId}`);
    return false;
  }
  sesiones.delete(userId);
  return true;
}
```

### Limpiar cache periódicamente

```js
const cache = new Map();

setInterval(() => {
  const ahora = Date.now();
  for (const [clave, { expira }] of cache) {
    if (ahora > expira) cache.delete(clave);
  }
}, 60_000);
```

## Cómo funciona por dentro

> [!info] Mantenimiento de size
> El motor actualiza `size` en O(1) en cada operación mutante: incrementa en `set` cuando la clave es nueva, decrementa en `delete` cuando la clave existía, y pone a cero en `clear`. No hay recuento diferido. La propiedad es un getter nativo de `Map.prototype` que retorna el valor interno directamente.

> [!tip] Buenas prácticas
> - Usar `map.has(k)` antes de `map.get(k)` cuando el valor puede ser falsy (`0`, `""`, `false`, `null`, `undefined`) — evita falsos negativos con `if (map.get(k))`.
> - Para limpiar un mapa que se reutiliza, preferir `map.clear()` sobre reasignar `map = new Map()` si hay otras referencias al mismo mapa.
> - Usar el valor de retorno de `delete` cuando el código necesita saber si la clave realmente existía.

> [!warning] Errores comunes
> - **Llamar `size` como función:** `map.size()` lanza `TypeError: map.size is not a function` — es una propiedad, no un método.
> - **`delete` sin `has` en flujos críticos:** `delete` no lanza error con claves ausentes, pero el valor de retorno `false` es la única señal de que nada fue eliminado. Ignorarlo puede ocultar bugs.
> - **Limpiar mapa con múltiples referencias:** si A y B referencian el mismo Map y A hace `a = new Map()`, B sigue apuntando al mapa original. Solo `clear()` vacía el objeto compartido.

## Notas relacionadas

- [[02 Map/index|Map — índice]]
- [[01 Creación y set/get|Creación, set y get]]
- [[03 Iteradores (keys, values, entries)]]
- [[04 Diferencias con Objetos]]
