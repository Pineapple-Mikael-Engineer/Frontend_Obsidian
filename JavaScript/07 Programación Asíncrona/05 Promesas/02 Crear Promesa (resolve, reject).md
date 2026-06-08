---
title: Crear una Promise — constructor, resolve y reject
aliases:
  - new Promise
  - Promise constructor
  - executor
  - promise resolve reject
tags:
  - javascript
  - api/clase
  - asincrono
objeto: Promise
tipo: clase
retorna: Promise
muta: false
asincrono: true
draft: false
---

# Crear una Promise

> [!definicion]
> `new Promise(executor)` crea una Promise nueva en estado `pending`. El **executor** es una función que se invoca síncronamente con dos argumentos: `resolve(valor)` para cumplir la promesa y `reject(razón)` para rechazarla. Solo la primera llamada a cualquiera de las dos tiene efecto; las siguientes se ignoran.

```js
const promesa = new Promise((resolve, reject) => {
  setTimeout(() => {
    const exito = Math.random() > 0.3;
    if (exito) resolve({ datos: [1, 2, 3] });
    else       reject(new Error('Operación fallida'));
  }, 500);
});

promesa
  .then(resultado => console.log(resultado.datos)) // → [1, 2, 3]
  .catch(err     => console.error(err.message));   // → 'Operación fallida'
```

## El executor

El executor se ejecuta **síncronamente** al llamar a `new Promise(...)`. Esto significa que el código antes y después de la creación de la Promise se ejecuta antes de que cualquier handler `.then` se encole.

```js
console.log('antes');

new Promise(resolve => {
  console.log('dentro del executor');  // síncrono
  resolve(1);
}).then(v => console.log('then:', v)); // asíncrono — microtask

console.log('después');

// → antes
// → dentro del executor
// → después
// → then: 1
```

Si el executor lanza una excepción, la Promise se rechaza automáticamente con esa excepción como razón:

```js
const p = new Promise((resolve, reject) => {
  throw new Error('error en executor');
  resolve('nunca llega'); // esta línea no se ejecuta
});

p.catch(err => console.error(err.message)); // → 'error en executor'
```

## resolve(valor) — comportamiento detallado

`resolve` no siempre produce `fulfilled` inmediatamente. El comportamiento depende del tipo de `valor`:

| Tipo de valor | Comportamiento |
|---|---|
| Primitivo o cualquier objeto no thenable | La Promise pasa a `fulfilled` con ese valor |
| Otra `Promise` | La Promise adopta el estado y resultado de la Promise argumento |
| Objeto thenable (tiene `.then`) | La Promise sigue el resultado del `.then` del thenable |

```js
// resolve con otra Promise — la promesa externa adopta el estado de la interna
const interna = new Promise(res => setTimeout(() => res(99), 1000));
const externa = new Promise(resolve => resolve(interna));

externa.then(v => console.log(v)); // → 99 (tras 1 segundo, no inmediatamente)
```

## reject(razón) — buenas prácticas

`reject` acepta cualquier valor, pero la convención es siempre pasar un objeto `Error` (o una instancia de una subclase). Rechazar con un string o un número dificulta el acceso al stack trace:

```js
// Correcto
reject(new Error('Tiempo de espera agotado'));

// Evitar
reject('error');         // sin stack trace
reject({ code: 500 });   // difícil de detectar con instanceof
```

## Promise.resolve y Promise.reject

Atajos estáticos para crear Promises ya asentadas:

```js
// Promise ya fulfilled
const ya = Promise.resolve(42);
ya.then(v => console.log(v)); // → 42

// Promise ya rejected
const falla = Promise.reject(new Error('ya rechazada'));
falla.catch(e => console.error(e.message)); // → 'ya rechazada'

// Promise.resolve con Promise — devuelve la misma Promise (no crea una nueva)
const p = new Promise(res => res(1));
Promise.resolve(p) === p; // → true
```

`Promise.resolve` es útil para normalizar valores que pueden ser síncronos o asíncronos:

```js
function obtenerConfig(valor) {
  return Promise.resolve(valor); // garantiza que siempre se retorna una Promise
}
```

## Receta: envolver una API de callback (promisify manual)

El patrón para convertir cualquier función error-first de Node.js a Promise:

```js
const fs = require('fs');

function readFileAsync(ruta, opciones) {
  return new Promise((resolve, reject) => {
    fs.readFile(ruta, opciones, (err, datos) => {
      if (err) reject(err);
      else     resolve(datos);
    });
  });
}

readFileAsync('./datos.json', 'utf8')
  .then(JSON.parse)
  .then(obj => console.log(obj))
  .catch(err => console.error('Error al leer:', err.message));
```

`util.promisify` de Node.js hace esto automáticamente para cualquier función que siga la convención error-first.

## Receta: timeout de operación asíncrona

```js
function conTimeout(promise, ms) {
  const limite = new Promise((_, reject) =>
    setTimeout(() => reject(new Error(`Timeout de ${ms}ms`)), ms)
  );
  return Promise.race([promise, limite]);
}

conTimeout(fetch('/api/lento'), 3000)
  .then(res => res.json())
  .catch(err => console.error(err.message)); // → 'Timeout de 3000ms' si tarda más
```

> [!tip]
> Siempre retorna la Promise creada dentro de funciones asíncronas para que el caller pueda encadenar handlers. Una Promise creada y no retornada es una promesa "flotante" cuyos rechazos no serán capturados por el caller — generan `unhandledrejection`.

> [!warning]
> El executor se ejecuta síncronamente, pero eso no significa que `resolve` pueda retornar el valor de forma síncrona. El valor solo está disponible dentro de un `.then` o con `await`. Intentar leer el resultado de una Promise de forma síncrona siempre produce `undefined` o `Promise {<pending>}`.

## Notas relacionadas

- [[index|Promesas — índice de la sección]]
- [[01 Estados (pending, fulfilled, rejected)|Estados de una Promise]]
- [[03 then|.then — encadenar sobre fulfilled]]
- [[09 Promise.race|Promise.race — útil para timeouts]]
