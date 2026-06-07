---
title: Array — includes, indexOf, lastIndexOf — Búsqueda por valor
aliases:
  - includes
  - indexOf
  - lastIndexOf
  - Array.includes
  - Array.indexOf
tags:
  - javascript
  - api/metodo
  - arrays
objeto: Array
tipo: metodo
retorna: boolean | number
muta: false
asincrono: false
draft: false
---

# Array — includes, indexOf, lastIndexOf

> [!definicion]
> Tres métodos de búsqueda lineal por valor. `includes(valor)` retorna `true`/`false` usando igualdad SameValueZero (detecta `NaN`). `indexOf(valor)` retorna el primer índice o `-1` usando igualdad estricta `===` (no detecta `NaN`). `lastIndexOf(valor)` igual que `indexOf` pero busca desde el final hacia el inicio.

## Firmas

| Método | Firma | Retorna | Algoritmo de igualdad |
|--------|-------|---------|----------------------|
| `includes` | `arr.includes(valor, desdeÍndice?)` | `boolean` | SameValueZero |
| `indexOf` | `arr.indexOf(valor, desdeÍndice?)` | `number` (índice o `-1`) | Igualdad estricta `===` |
| `lastIndexOf` | `arr.lastIndexOf(valor, desdeÍndice?)` | `number` (índice o `-1`) | Igualdad estricta `===` |

Todos tienen complejidad O(n). **No mutan.**

## includes — comprobar existencia

```js
const frutas = ["manzana", "pera", "uva", NaN];

frutas.includes("pera");    // → true
frutas.includes("melón");   // → false
frutas.includes(NaN);       // → true   ← SameValueZero detecta NaN

// Con desdeÍndice
frutas.includes("manzana", 1); // → false  (busca desde índice 1)
frutas.includes("uva", -2);    // → true   (desde el penúltimo)
```

## indexOf — obtener la posición

```js
const arr = [10, 20, 30, 20, 10];

arr.indexOf(20);       // → 1   (primera ocurrencia)
arr.indexOf(20, 2);    // → 3   (busca desde índice 2)
arr.indexOf(99);       // → -1  (no encontrado)
arr.indexOf(NaN);      // → -1  ← !!! NaN !== NaN en ===

// Patrón de comprobación de existencia con indexOf (legado)
if (arr.indexOf(20) !== -1) { /* existe */ }
// Mejor hoy:
if (arr.includes(20)) { /* existe */ }
```

## lastIndexOf — última ocurrencia

```js
const arr = ["a", "b", "c", "b", "a"];

arr.lastIndexOf("b");     // → 3  (desde el final, primer "b" encontrado)
arr.lastIndexOf("b", 2);  // → 1  (busca hacia atrás desde índice 2)
arr.lastIndexOf("z");     // → -1
```

## Tabla comparativa

| Pregunta | Usar | Retorna |
|----------|------|---------|
| ¿Existe el valor? | `includes` | `boolean` |
| ¿En qué índice está? | `indexOf` | `number` |
| ¿Dónde está la última copia? | `lastIndexOf` | `number` |
| ¿Existe con condición compleja? | `find` / `findIndex` | elemento / índice |
| ¿Existe basado en objeto? | `some` | `boolean` |

## La trampa de `NaN`

`NaN` no es igual a sí mismo bajo `===`. Por eso `indexOf` no puede encontrarlo, pero `includes` sí (usa SameValueZero que maneja este caso especial):

```js
const arr = [1, NaN, 3];

arr.indexOf(NaN);   // → -1  (falla: NaN !== NaN)
arr.includes(NaN);  // → true (SameValueZero: NaN === NaN)
```

> [!warning] Errores comunes
> - Usar `indexOf` para comprobar existencia de `NaN` en el array — siempre retorna `-1`. Usar `includes` en su lugar.
> - Confundir `desdeÍndice` negativo: en `indexOf`, el negativo se trata como `Math.max(0, length + desdeÍndice)`. En `lastIndexOf`, el negativo se trata como `length + desdeÍndice`.
> - `indexOf` con objetos: compara por referencia, no por valor estructural. `[{a:1}].indexOf({a:1})` → `-1` porque son objetos distintos.
> - Usar `if (arr.indexOf(x))` en lugar de `if (arr.indexOf(x) !== -1)` — el índice `0` es falsy aunque el elemento exista.

## Recetas comunes

```js
// Comprobar si un valor está en una lista de permitidos
const permitidos = ["admin", "editor", "viewer"];
if (permitidos.includes(rol)) { /* autorizado */ }

// Encontrar y eliminar la primera ocurrencia
function eliminarPrimero(arr, val) {
  const idx = arr.indexOf(val);
  if (idx !== -1) arr.splice(idx, 1);
}

// Encontrar todas las posiciones de un valor
function todasLasPosiciones(arr, val) {
  const posiciones = [];
  let idx = arr.indexOf(val);
  while (idx !== -1) {
    posiciones.push(idx);
    idx = arr.indexOf(val, idx + 1);
  }
  return posiciones;
}
todasLasPosiciones([1, 2, 1, 3, 1], 1); // → [0, 2, 4]
```

> [!tip] Buenas prácticas
> - Preferir `includes` para comprobaciones de existencia — semántica más clara que `!== -1`.
> - Usar `indexOf` cuando se necesita el índice para luego pasarlo a `splice` u otras operaciones posicionales.
> - Para búsqueda con condición compleja (objetos, predicados), usar `find`/`findIndex` en lugar de `indexOf`.

## Notas relacionadas

- [[05 splice]]
- [[10 sort]]
- [[index|Arrays]]
