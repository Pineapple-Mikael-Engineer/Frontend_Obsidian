---
title: while — Bucle con condición previa
aliases:
  - while loop
  - bucle while
tags:
  - javascript
  - api/concepto
  - control-flujo
objeto: global
tipo: concepto
draft: false
---

# while

> [!definicion]
> `while` ejecuta un bloque de código repetidamente mientras una condición sea verdadera. La condición se evalúa **antes** de cada iteración: si es falsa desde el inicio, el cuerpo nunca se ejecuta. Sintaxis: `while (condición) { cuerpo }`.

```js
let i = 0;
while (i < 3) {
  console.log(i); // 0, 1, 2
  i++;
}
// Condición falsa desde el inicio → cuerpo nunca ejecutado
let j = 10;
while (j < 3) {
  console.log("nunca");
}
```

## Cómo funciona por dentro

El motor evalúa la condición con la conversión interna **ToBoolean** (equivalente a `!!condición`). Si el resultado es `true`, ejecuta el cuerpo y repite la evaluación. Si es `false`, sale del bucle y continúa con la instrucción siguiente. No hay limite intrínseco de iteraciones: la responsabilidad de terminar recae en el programador.

## Cuándo usar `while`

`while` es el bucle natural cuando la cantidad de iteraciones no se conoce de antemano y depende de un estado que evoluciona dentro del cuerpo: consumir una cola, leer hasta EOF, esperar una condición de red, implementar un reintento con límite.

El [[03 for Clásico]] es preferible cuando la cantidad de iteraciones es conocida o cuando se itera sobre un índice numérico explícito.

## Bucle infinito con `break` explícito

`while (true)` crea un bucle que solo termina con una instrucción `break` (o `return`/`throw`). Es el patrón estándar para event loops simplificados, servidores de sockets y parsers que leen hasta un token centinela.

```js
// Reintento con límite — event loop simplificado
function fetchWithRetry(url, maxRetries) {
  let attempts = 0;
  while (true) {
    try {
      const result = tryFetch(url);
      return result;            // éxito: sale del bucle
    } catch (err) {
      attempts++;
      if (attempts >= maxRetries) throw err;
    }
  }
}
```

```js
// Parser de tokens — sale cuando encuentra el centinela
function readUntilSemicolon(tokens) {
  const result = [];
  let i = 0;
  while (true) {
    const tok = tokens[i++];
    if (tok === ";" || tok === undefined) break;
    result.push(tok);
  }
  return result;
}
```

## Recetas comunes

**Leer un stream línea a línea (Node.js)**

```js
import { createInterface } from "readline";
import { createReadStream } from "fs";

const rl = createInterface({ input: createReadStream("data.txt") });
const lines = [];

for await (const line of rl) {   // ver for await...of
  lines.push(line);
}
```

**Polling hasta condición con backoff**

```js
async function waitForReady(checkFn, maxMs = 5000) {
  const start = Date.now();
  while (!checkFn()) {
    if (Date.now() - start > maxMs) throw new Error("Timeout");
    await new Promise(r => setTimeout(r, 200));
  }
}
```

> [!tip] Buenas prácticas
> - Asegurar que la condición evolucione hacia `false` dentro del cuerpo para evitar bucles infinitos accidentales.
> - Preferir `for...of` cuando se itera sobre una colección — `while` con índice manual es más propenso a errores de off-by-one.
> - Cuando se usa `while (true)`, documentar el criterio de salida junto al `break`.

> [!warning] Errores comunes
> **Bucle infinito accidental** — olvidar actualizar la variable de condición dentro del cuerpo:
> ```js
> let i = 0;
> while (i < 5) {
>   console.log(i);
>   // falta i++ → bucle infinito
> }
> ```
> **Condición con efecto secundario no intencionado** — usar asignación (`=`) en lugar de comparación (`===`) en la condición. En modo estricto `"use strict"` el linter lo señalará como error.

## Notas relacionadas

- [[06 Bucles/index | Bucles]]
- [[02 do...while]]
- [[03 for Clásico]]
- [[07 break y continue]]
