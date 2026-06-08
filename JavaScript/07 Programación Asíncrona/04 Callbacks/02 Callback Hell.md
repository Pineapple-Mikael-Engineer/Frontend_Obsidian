---
title: Callback Hell — pyramid of doom en JavaScript
aliases:
  - callback hell
  - pyramid of doom
  - anidamiento de callbacks
tags:
  - javascript
  - api/concepto
  - asincrono
draft: false
---

# Callback Hell

> [!definicion]
> **Callback Hell** (también "pyramid of doom") es el patrón de anidamiento profundo de callbacks asíncronos que surge cuando múltiples operaciones asíncronas dependen unas de otras en secuencia. El código resultante tiene forma de pirámide hacia la derecha, es difícil de leer, de mantener y de depurar.

```js
getUser(id, (user) => {
  getPosts(user.id, (posts) => {
    getComments(posts[0].id, (comments) => {
      getLikes(comments[0].id, (likes) => {
        // nivel 5 — el código ya es ilegible
        actualizarUI(user, posts, comments, likes);
      }, handleError);
    }, handleError);
  }, handleError);
}, handleError);
```

Cada nivel añade dos espacios de indentación mínimos (más la lógica interna), y el cierre de llaves y paréntesis se acumula al final hasta volverse imposible de rastrear.

## Por qué ocurre

Las operaciones asíncronas que dependen del resultado de la anterior no pueden expresarse linealmente con callbacks — el único lugar donde `user` está disponible es dentro del callback de `getUser`, por lo que `getPosts` tiene que anidarse allí, y así sucesivamente:

```js
// Intento incorrecto — resultado no disponible aquí
const user = getUser(id, callback); // getUser no devuelve el user
const posts = getPosts(user.id);    // user es undefined
```

```js
// Correcto con callbacks — pero genera anidamiento
getUser(id, (user) => {
  // user disponible solo aquí
  getPosts(user.id, (posts) => {
    // posts disponible solo aquí
  });
});
```

## Problemas concretos

**Legibilidad**: el flujo lógico se lee en diagonal, no de arriba a abajo. Una secuencia de 5 operaciones requiere 5 niveles de indentación.

**Manejo de errores inconsistente**: cada nivel necesita su propio `handleError`. Es fácil olvidar uno, duplicar código de manejo, o tener distintas estrategias de error en distintos niveles. No hay un punto único de captura de errores.

**Refactorización difícil**: extraer o reordenar pasos es complicado porque las variables del scope de un nivel no son accesibles desde fuera de su callback.

**Stack traces confusos**: cuando un error ocurre en el nivel 4, el stack trace no muestra la cadena de llamadas legible — muestra frames anónimos o referencias internas de la librería.

**No composable**: no hay forma estándar de ejecutar dos operaciones en paralelo y esperar ambas sin inventar mecanismos ad-hoc (`Promise.all` no existe en el mundo de callbacks).

## Soluciones históricas (antes de Promises)

### Callbacks con nombre

Sacar las funciones anónimas inline y darles nombre rompe visualmente la pirámide:

```js
function alObtenerLikes(likes) {
  actualizarUI(likes);
}

function alObtenerComments(comments) {
  getLikes(comments[0].id, alObtenerLikes, handleError);
}

function alObtenerPosts(posts) {
  getComments(posts[0].id, alObtenerComments, handleError);
}

function alObtenerUser(user) {
  getPosts(user.id, alObtenerPosts, handleError);
}

getUser(id, alObtenerUser, handleError);
```

El código se vuelve lineal visualmente, pero el flujo real sigue siendo de abajo hacia arriba (hay que leer `alObtenerLikes` para entender qué hace `alObtenerUser`). Además los nombres pierden contexto si las funciones se usan en distintos lugares.

### Modularización

Mover cada nivel a su propio módulo o función de mayor nivel. Ayuda a la organización, pero no elimina el problema de composición — sigue siendo necesario encadenar callbacks.

## Por qué los callbacks llevaron a las Promises

Los dos problemas fundamentales que las Promises resuelven:

**Inversión de control**: con un callback cedes el control de cuándo y cómo se llama tu función. Con una Promise, recibes un objeto que puedes encadenar, inspeccionar y pasar — tú controlas el flujo.

**Trustability**: con callbacks, confías en que la librería llame tu callback exactamente una vez, con los argumentos correctos, manejando errores apropiadamente. Una Promise garantiza formalmente que se resuelve o rechaza exactamente una vez, y que el estado no cambia.

```js
// Callbacks — inversión de control
libreria.operar(miCallback); // libreria decide todo

// Promises — el caller controla el flujo
libreria.operar()
  .then(resultado => procesarResultado(resultado))
  .then(procesado => guardar(procesado))
  .catch(err => manejarError(err)); // un solo punto de manejo de errores
```

La equivalencia en async/await hace que el código asíncrono se lea como síncrono, eliminando tanto la pirámide como la verbosidad de `.then()`:

```js
async function cargarTodo(id) {
  const user     = await getUser(id);
  const posts    = await getPosts(user.id);
  const comments = await getComments(posts[0].id);
  const likes    = await getLikes(comments[0].id);
  actualizarUI(user, posts, comments, likes);
}
```

> [!tip] Cuándo los callbacks anidados son aceptables
> Un solo nivel de anidamiento de callbacks (un callback dentro de otro) es completamente legible y es el patrón normal para event listeners con lógica inline, o `setTimeout` con una única acción diferida. El problema surge a partir del tercer o cuarto nivel, especialmente cuando la lógica de negocio en cada nivel es no trivial.

> [!warning] Promisify no siempre es suficiente
> Convertir APIs error-first a Promises con `util.promisify` es el primer paso, pero si el diseño original usa callbacks múltiples (progress callbacks, streaming) no hay equivalencia directa con Promises — esos casos requieren Streams o EventEmitters.

## Notas relacionadas

- [[index|Callbacks — índice de la sección]]
- [[01 Patrón Básico|Patrón Básico — qué es un callback]]
- [[03 Error-First Pattern|Error-First Pattern — convención de Node.js]]
