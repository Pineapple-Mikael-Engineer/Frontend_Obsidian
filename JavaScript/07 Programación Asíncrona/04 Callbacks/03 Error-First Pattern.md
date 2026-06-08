---
title: Error-First Pattern — convención de callbacks en Node.js
aliases:
  - error-first callback
  - node callback convention
  - errback
tags:
  - javascript
  - api/concepto
  - asincrono
draft: false
---

# Error-First Pattern

> [!definicion]
> El **Error-First Pattern** (también "errback pattern") es la convención estándar de Node.js para callbacks asíncronos: el **primer parámetro del callback es siempre el error** (`null` si no hubo error, un objeto `Error` si lo hubo). Los datos del resultado van en los parámetros siguientes. Esta convención está presente en toda la API nativa de Node.js: `fs`, `http`, `crypto`, `dns`.

```js
fs.readFile("archivo.txt", "utf8", (err, datos) => {
  if (err) {
    console.error("Error al leer:", err.message);
    return; // salir del callback — no procesar datos inválidos
  }
  console.log(datos);
});
```

## Por qué el error va primero

Si el error fuera el último parámetro, sería el más fácil de omitir — basta con no declararlo en la firma del callback. Al ponerlo primero, el desarrollador tiene que escribirlo explícitamente y es visualmente imposible ignorarlo. La convención **obliga a confrontar el error** antes de acceder al resultado.

```js
// Si el error fuera el último — fácil de olvidar:
fs.readFile("archivo.txt", "utf8", (datos, err) => {
  // err está ahí, pero visualmente parece secundario
  procesarDatos(datos); // si hubo error, datos es undefined
});

// Error primero — obliga a la verificación:
fs.readFile("archivo.txt", "utf8", (err, datos) => {
  if (err) return manejarError(err);
  procesarDatos(datos); // solo llega aquí si no hubo error
});
```

## Convención de parámetros

| Parámetro | Tipo en error | Tipo en éxito | Descripción |
|---|---|---|---|
| `err` (1er param) | objeto `Error` | `null` | Error ocurrido, o null si todo fue bien |
| `result` (2o param) | `undefined` | cualquier tipo | Resultado de la operación |
| params extra | `undefined` | cualquier tipo | Resultados adicionales (poco común) |

## Ejemplo con múltiples errores posibles

```js
function leerYParsear(ruta, callback) {
  fs.readFile(ruta, "utf8", (err, texto) => {
    if (err) return callback(err); // propagar el error hacia arriba

    let datos;
    try {
      datos = JSON.parse(texto);
    } catch (parseError) {
      return callback(new Error(`JSON inválido en ${ruta}: ${parseError.message}`));
    }

    callback(null, datos); // éxito: null como primer argumento
  });
}

leerYParsear("config.json", (err, config) => {
  if (err) {
    console.error(err.message);
    process.exit(1);
  }
  inicializar(config);
});
```

Patrón clave: al propagar el error hacia el caller, usar `return callback(err)` — el `return` evita que el código continúe ejecutándose después de llamar al callback.

## util.promisify — convertir error-first a Promise

Node.js incluye `util.promisify` para convertir cualquier función que siga la convención error-first en una función que devuelve una Promise:

```js
const { promisify } = require("util");
const fs = require("fs");

const readFile = promisify(fs.readFile);

// Ahora readFile devuelve una Promise:
readFile("archivo.txt", "utf8")
  .then(datos => console.log(datos))
  .catch(err => console.error(err.message));

// O con async/await:
async function main() {
  try {
    const datos = await readFile("archivo.txt", "utf8");
    console.log(datos);
  } catch (err) {
    console.error(err.message);
  }
}
```

Node.js moderno también expone versiones promisificadas directamente en `fs/promises`, `dns/promises`, etc., sin necesidad de `promisify` manual.

## Cómo funciona promisify por dentro

`util.promisify` envuelve la función original en una nueva que devuelve una Promise. La implementación conceptual:

```js
function promisify(fn) {
  return function (...args) {
    return new Promise((resolve, reject) => {
      fn(...args, (err, result) => {
        if (err) reject(err);
        else resolve(result);
      });
    });
  };
}
```

Si la función usa el símbolo especial `util.promisify.custom`, puede definir su propia implementación promisificada (útil para funciones con múltiples valores de éxito).

## callbackify — el proceso inverso

`util.callbackify` convierte una función async (que devuelve Promise) en una función con callback error-first. Útil para exponer APIs async como error-first en contextos que las requieren:

```js
const { callbackify } = require("util");

async function obtenerDatos(id) {
  const respuesta = await fetch(`/api/${id}`);
  return respuesta.json();
}

const obtenerDatosCallback = callbackify(obtenerDatos);

obtenerDatosCallback(42, (err, datos) => {
  if (err) throw err;
  console.log(datos);
});
```

> [!tip] fs/promises como alternativa directa
> Para el módulo `fs` y la mayoría de las APIs nativas de Node.js moderno (v10+), existe el namespace `/promises` que ya expone versiones Promise nativas sin usar `promisify`: `const { readFile } = require("fs/promises")` o `import { readFile } from "fs/promises"`. Son preferibles a `promisify` porque pueden tener optimizaciones internas adicionales.

> [!warning] promisify solo funciona con la convención error-first estricta
> `util.promisify` asume que el callback tiene la firma `(err, result)`. Si una función usa una convención diferente (múltiples argumentos de éxito, callback sin error como primer parámetro), `promisify` no funciona correctamente — hay que envolverla manualmente en una Promise.

## Notas relacionadas

- [[index|Callbacks — índice de la sección]]
- [[01 Patrón Básico|Patrón Básico — qué es un callback]]
- [[02 Callback Hell|Callback Hell — pyramid of doom]]
