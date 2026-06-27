---
title: for await...of — Iterar iterables asíncronos
aliases:
  - for await of
  - for await...of loop
  - async iteration
tags:
  - javascript
  - api/concepto
  - control-flujo
  - async
objeto: global
tipo: concepto
asincrono: true
draft: false
order: 6
---

# for await...of

> [!definicion]
> `for await...of` itera un **iterable asíncrono** (objeto con `Symbol.asyncIterator`) o un iterable ordinario cuyas posiciones son Promises, awaiteando cada valor antes de continuar. Solo puede usarse dentro de una función `async`. Sintaxis: `for await (const valor of iterableAsincrono) { cuerpo }`.

```js
async function main() {
  const urls = [fetch("/api/a"), fetch("/api/b"), fetch("/api/c")];

  for await (const response of urls) {
    const data = await response.json();
    console.log(data); // procesa en orden: a, luego b, luego c
  }
}
```

## Cómo funciona por dentro

El motor llama a `iterable[Symbol.asyncIterator]()` para obtener el iterador asíncrono. En cada iteración llama a `iterador.next()`, que retorna una **Promise** de `{ value, done }`. El `await` implícito suspende la función hasta que esa Promise se resuelva. Cuando `done` es `true`, sale del bucle.

Si el iterable no tiene `Symbol.asyncIterator` pero sí `Symbol.iterator`, `for await...of` lo trata como iterable síncrono y awaitea cada `value` individualmente (lo que permite iterar un array de Promises).

```
iterador.next() → Promise<{value, done}>
        ↓
     await (suspender)
        ↓
   {value: X, done: false} → cuerpo con X
        ↓
   iterador.next() → ...
```

## Ejemplo con ReadableStream (fetch body)

El body de una respuesta fetch es un `ReadableStream` que implementa `Symbol.asyncIterator` en entornos modernos:

```js
async function procesarStream(url) {
  const response = await fetch(url);
  const decoder = new TextDecoder();
  let texto = "";

  for await (const chunk of response.body) {
    texto += decoder.decode(chunk, { stream: true });
  }
  texto += decoder.decode(); // flush final
  return texto;
}
```

## Ejemplo con iterable asíncrono personalizado

```js
async function* paginacion(endpoint) {
  let pagina = 1;
  while (true) {
    const res = await fetch(`${endpoint}?page=${pagina}`);
    const datos = await res.json();
    if (datos.length === 0) break;
    yield datos;
    pagina++;
  }
}

async function main() {
  for await (const pagina of paginacion("/api/productos")) {
    console.log(`${pagina.length} productos en esta página`);
    pagina.forEach(p => procesar(p));
  }
}
```

## Ejemplo con Node.js streams (readline)

Node.js expone los streams de líneas como iterables asíncronos:

```js
import { createInterface } from "readline";
import { createReadStream } from "fs";

async function contarLineas(archivo) {
  const rl = createInterface({ input: createReadStream(archivo) });
  let lineas = 0;
  for await (const linea of rl) {
    lineas++;
  }
  return lineas;
}
```

## `for await...of` vs `Promise.all` + `for...of`

| Aspecto | `for await...of` | `Promise.all` + `for...of` |
|---|---|---|
| Procesamiento | secuencial (una a la vez) | paralelo (todas a la vez) |
| Inicio | espera a que termine la anterior | todas inician simultáneamente |
| Uso de recursos | menor pico de memoria | mayor pico, menor latencia total |
| Manejo de errores | un fallo para el bucle | `Promise.all` rechaza en el primer fallo |

```js
// Secuencial — for await...of: total ≈ suma de tiempos individuales
for await (const result of promises) { procesar(result); }

// Paralelo — Promise.all: total ≈ el más lento
const results = await Promise.all(promises);
for (const result of results) { procesar(result); }
```

Usar `for await...of` cuando el orden importa, cuando los recursos son limitados (no se puede lanzar todo a la vez) o cuando se consume un stream infinito. Usar `Promise.all` cuando se quiere minimizar la latencia total y los recursos lo permiten.

## Manejo de errores

```js
async function main() {
  try {
    for await (const valor of iterableAsincrono) {
      procesar(valor);
    }
  } catch (err) {
    // captura errores tanto del iterador como del cuerpo
    console.error("Error en la iteración:", err);
  } finally {
    // cleanup — el motor llama a iterador.return() al salir (break, throw, return)
    await cerrarConexion();
  }
}
```

> [!tip] Buenas prácticas
> - Usar `for await...of` para streams y paginación donde el tamaño total es desconocido — es más eficiente en memoria que `Promise.all` con colecciones grandes.
> - Siempre envolver en `try/catch` — un error en cualquier iteración sale del bucle y se propaga.
> - Para paralelismo limitado (N en paralelo), combinar con una cola de semáforo en lugar de `Promise.all` puro.

> [!warning] Errores comunes
> **Usarlo fuera de `async`** — `for await...of` fuera de un contexto `async` (o fuera de un módulo con top-level await) es un `SyntaxError`.
> **Confundirlo con `for...of` de Promises** — `for...of` sobre un array de Promises itera las Promises sin awaitearlas (produce objetos Promise, no sus valores). `for await...of` es necesario para awaitear cada una.
> ```js
> const promises = [fetch("/a"), fetch("/b")];
> for (const p of promises) {
>   console.log(p); // Promise { <pending> } — no awaiteado
> }
> for await (const p of promises) {
>   console.log(p); // Response — awaiteado correctamente
> }
> ```

## Notas relacionadas

- [[06 Bucles/index | Bucles]]
- [[05 for...of]]
- [[JavaScript/07 Programación Asíncrona/06 Async / Await/index | Async / Await]]
- [[JavaScript/07 Programación Asíncrona/06 Async / Await/05 Bucles Asíncronos (for await...of) | Bucles Asíncronos (for await...of)]]
- [[JavaScript/07 Programación Asíncrona/05 Promesas/index | Promesas]]
