---
title: Rest Parameters — Argumentos restantes como Array real
aliases:
  - rest parameters
  - rest params
  - parámetros rest
  - operador rest
tags:
  - javascript
  - api/concepto
  - funciones
objeto: global
tipo: concepto
draft: false
order: 4
---

# Rest Parameters

> [!definicion]
> Los **rest parameters** (`...nombre`) recogen todos los argumentos sobrantes (los que no tienen parámetro formal explícito) en un **Array real**. Deben declararse como el último parámetro de la función. A diferencia del objeto `arguments`, son un Array verdadero con todos sus métodos y funcionan en arrow functions.

```js
function sumar(...numeros) {
  return numeros.reduce((acc, n) => acc + n, 0);
}

sumar(1, 2, 3);       // → 6
sumar(10, 20, 30, 40); // → 100
sumar();              // → 0  (numeros = [])
```

## Sintaxis

```js
function f(a, b, ...resto) {
  // a y b reciben los dos primeros argumentos
  // resto es un Array con los argumentos a partir del índice 2
}
```

`...resto` solo puede aparecer **una vez** y siempre como **último parámetro**. Colocarlo en otra posición es SyntaxError.

## Rest vs `arguments`

| Característica | `...rest` | `arguments` |
|---|---|---|
| Tipo | Array real (`instanceof Array`) | objeto array-like |
| Métodos de Array | sí (`map`, `filter`, `reduce`…) | no |
| Arrow functions | sí | no (heredan del externo) |
| Contenido | solo los argumentos "sobrantes" | todos los argumentos |
| Parámetros explícitos | no los incluye | los incluye todos |
| ES | ES6+ | ES1 (legacy) |

```js
function comparar(primero, ...resto) {
  console.log(primero); // primer argumento
  console.log(resto);   // Array con el resto
}

comparar("a", "b", "c", "d");
// → "a"
// → ["b", "c", "d"]
```

## Con parámetros explícitos previos

Los parámetros antes del rest se asignan normalmente; `...resto` captura lo que sobre:

```js
function registrarEvento(tipo, fecha, ...detalles) {
  return {
    tipo,
    fecha,
    detalles, // Array — puede estar vacío
  };
}

registrarEvento("click", "2026-06-07", "button#submit", "user:42");
// → { tipo: "click", fecha: "2026-06-07", detalles: ["button#submit", "user:42"] }

registrarEvento("load", "2026-06-07");
// → { tipo: "load", fecha: "2026-06-07", detalles: [] }
```

## Efecto en `fn.length`

El rest parameter **no cuenta** en `fn.length`. Solo los parámetros antes del rest (y antes del primer default) se contabilizan:

```js
function f(a, b, ...resto) {}
f.length; // → 2

function g(...todos) {}
g.length; // → 0
```

## Recetas comunes

**Función variádica de log con prefijo**:

```js
function logConPrefijo(prefijo, ...mensajes) {
  mensajes.forEach(msg => console.log(`[${prefijo}] ${msg}`));
}

logConPrefijo("ERROR", "Conexión fallida", "Reintentando en 5s");
// [ERROR] Conexión fallida
// [ERROR] Reintentando en 5s
```

**Merge de objetos** — composición de rest con spread:

```js
function merge(base, ...overrides) {
  return Object.assign({}, base, ...overrides);
}

const config = merge(
  { host: "localhost", puerto: 3000, debug: false },
  { puerto: 8080 },
  { debug: true }
);
// → { host: "localhost", puerto: 8080, debug: true }
```

**Wrapper que reenvía argumentos**:

```js
function conLog(fn) {
  return function(...args) {
    console.log("Llamada con:", args);
    const resultado = fn(...args);
    console.log("Resultado:", resultado);
    return resultado;
  };
}

const sumarConLog = conLog((a, b) => a + b);
sumarConLog(3, 7);
// Llamada con: [3, 7]
// Resultado: 10
```

**Desestructuración del rest** — el array puede desestructurarse:

```js
function cabezaCola(primero, segundo, ...cola) {
  return { primero, segundo, cola };
}

cabezaCola(1, 2, 3, 4, 5);
// → { primero: 1, segundo: 2, cola: [3, 4, 5] }
```

## Cómo funciona por dentro

El motor agrupa los argumentos restantes (a partir del índice correspondiente) y construye un Array real mediante `Array.from` sobre el segmento del objeto `arguments` subyacente. Es azúcar sintáctico sobre:

```js
// Equivalente pre-ES6
function f(a, b) {
  var resto = Array.prototype.slice.call(arguments, 2);
}
```

En motores modernos (V8, SpiderMonkey), el rest parameter tiene optimizaciones específicas: el motor puede evitar materializar el objeto `arguments` completo cuando solo se usa el rest.

> [!tip] Buenas prácticas
> - Preferir rest parameters sobre `arguments` en código nuevo: son un Array real, funcionan en arrows y tienen semántica explícita en la firma.
> - Nombrar el rest de forma descriptiva (`...args` es genérico; `...valores`, `...oyentes`, `...rutas` comunican intención).
> - Si la función tiene parámetros obligatorios, declararlos antes del rest para que aparezcan en `fn.length` y en el autocompletado.

> [!warning] Errores comunes
> **Rest no en última posición** — es SyntaxError:
> ```js
> function f(...a, b) {} // SyntaxError: Rest element must be last element
> ```
> **Mezclar rest con `arguments`** — funciona técnicamente pero es confuso y no tiene sentido práctico:
> ```js
> function f(...args) {
>   // arguments no existe en arrows; en funciones normales existe pero args ya hace su trabajo
>   // usar uno u otro, nunca los dos
> }
> ```
> **Spread en llamada ≠ rest en definición** — el operador `...` tiene dos roles distintos: en la firma de una función es rest (recoge); en una llamada o expresión es spread (expande):
> ```js
> function f(...args) {}     // rest: recoge argumentos en un array
> f(...[1, 2, 3]);           // spread: expande el array en argumentos separados
> ```

## Notas relacionadas

- [[07 Funciones (básico)/index | Funciones (básico)]]
- [[05 Objeto arguments]]
- [[03 Parámetros por Defecto]]
- [[JavaScript/03 Programación Funcional/05 Inmutabilidad Práctica/01 Spread Operator | Spread Operator]]
