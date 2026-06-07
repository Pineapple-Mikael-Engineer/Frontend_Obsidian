---
title: break y continue — Control de flujo dentro de bucles
aliases:
  - break
  - continue
  - break statement
  - continue statement
tags:
  - javascript
  - api/concepto
  - control-flujo
objeto: global
tipo: concepto
draft: false
---

# break y continue

> [!definicion]
> `break` sale inmediatamente del bucle (o `switch`) que lo contiene. `continue` salta al inicio de la siguiente iteración, omitiendo el resto del cuerpo actual. Sin etiqueta, ambos afectan únicamente al **bucle más interno** que los contiene.

```js
// break — sale al encontrar el primer elemento par
const nums = [1, 3, 4, 7, 8];
for (const n of nums) {
  if (n % 2 === 0) {
    console.log("Encontrado:", n); // "Encontrado: 4"
    break;
  }
}

// continue — salta los impares, imprime solo pares
for (const n of nums) {
  if (n % 2 !== 0) continue;
  console.log(n); // 4, 8
}
```

## `break`

`break` transfiere el control a la instrucción inmediatamente posterior al bucle (o `switch`). En bucles anidados, sale solo del bucle más interno salvo que se use con etiqueta (ver [[08 Labeled Statements]]).

**Patrón de búsqueda lineal:**

```js
function buscar(arr, objetivo) {
  let posicion = -1;
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === objetivo) {
      posicion = i;
      break; // inútil seguir buscando
    }
  }
  return posicion;
}
```

**`break` en `switch`:**

`break` dentro de un `switch` sale del `switch`, no de un bucle exterior. Si el `switch` está dentro de un bucle y se quiere salir del bucle, se necesita una etiqueta o una variable de control.

```js
for (let i = 0; i < 10; i++) {
  switch (i % 3) {
    case 0:
      console.log("divisible por 3");
      break; // sale del switch, NO del for
    case 1:
      console.log("resto 1");
      break;
  }
}
```

**`break` en `while (true)` — patrón event loop:**

```js
while (true) {
  const evento = cola.dequeue();
  if (evento === null) break; // fin de la cola
  procesar(evento);
}
```

## `continue`

`continue` salta el resto del cuerpo de la iteración actual y pasa a evaluar la condición (en `while`/`do...while`) o la actualización (en `for` clásico).

**Filtrado de elementos inválidos:**

```js
const datos = [1, null, 3, undefined, 5, NaN, 7];
const validos = [];
for (const d of datos) {
  if (d == null || Number.isNaN(d)) continue; // salta nulos/NaN
  validos.push(d);
}
console.log(validos); // [1, 3, 5, 7]
```

**Efecto de `continue` en cada tipo de bucle:**

```js
// while — continue salta a la evaluación de la condición
let i = 0;
while (i < 5) {
  i++;
  if (i === 3) continue; // la actualización ya se hizo, funciona bien
  console.log(i); // 1, 2, 4, 5
}

// for clásico — continue salta al paso de actualización (i++)
for (let j = 0; j < 5; j++) {
  if (j === 2) continue;
  console.log(j); // 0, 1, 3, 4
}

// TRAMPA: continue en while sin actualizar ANTES de continue → bucle infinito
let k = 0;
while (k < 5) {
  if (k === 2) continue; // k nunca llega a 3 → bucle infinito
  console.log(k);
  k++;
}
```

## Comparación con métodos funcionales

| Imperativo | Funcional equivalente |
|---|---|
| `for` + `break` (buscar primero) | `Array.prototype.find` |
| `for` + `break` (buscar índice) | `Array.prototype.findIndex` |
| `for` + `continue` (filtrar) | `Array.prototype.filter` |
| `for` + `break` (comprobar existencia) | `Array.prototype.some` / `every` |

```js
// Buscar con break — equivalente funcional con find
const arr = [1, 3, 4, 7, 8];

// Imperativo
let encontrado;
for (const n of arr) {
  if (n % 2 === 0) { encontrado = n; break; }
}

// Declarativo
const encontrado2 = arr.find(n => n % 2 === 0); // 4
```

**Cuándo usar `break`/`continue` vs métodos funcionales:**

Los métodos funcionales (`find`, `filter`, `some`) son más declarativos y legibles en la mayoría de casos. `break`/`continue` son preferibles cuando la lógica de filtrado es compleja, cuando se itera sobre un iterable no-array (generadores, streams), o cuando el bucle realiza múltiples operaciones y salir temprano implica un ahorro real de trabajo.

> [!tip] Buenas prácticas
> - Usar `break` para búsqueda sobre colecciones grandes — evita procesar elementos innecesarios.
> - En `while` con `continue`, asegurar que la actualización de la variable de condición ocurra **antes** del `continue`, o nunca se actualizará.
> - Preferir métodos funcionales (`find`, `filter`) para colecciones de datos simples; reservar `break`/`continue` para flujos con lógica compleja o iterables no-array.

> [!warning] Errores comunes
> **`break` en `switch` dentro de un bucle** — `break` sale del `switch`, no del bucle. Para salir del bucle desde dentro de un `switch`, usar una etiqueta o una variable de flag.
> **`continue` antes de la actualización en `while`** — si la variable de condición se actualiza después del `continue`, el bucle nunca progresa y se vuelve infinito.
> **`break` en callbacks** — `break` no funciona en callbacks de métodos como `forEach`. No es posible salir de un `forEach` con `break`; hay que usar `for...of` o `some`/`every`.
> ```js
> [1, 2, 3].forEach(n => {
>   if (n === 2) break; // SyntaxError: Illegal break statement
> });
> ```

## Notas relacionadas

- [[06 Bucles/index | Bucles]]
- [[08 Labeled Statements]]
- [[01 while]]
- [[03 for Clásico]]
- [[05 for...of]]
- [[JavaScript/05 Manipulación del DOM/01 Selección de Elementos/index | Selección de Elementos]]
