---
title: Patrón Básico de Callbacks en JavaScript
aliases:
  - callback básico
  - higher-order function callback
tags:
  - javascript
  - api/concepto
  - asincrono
draft: false
---

# Patrón Básico de Callbacks

> [!definicion]
> Un **callback** es cualquier función pasada como argumento a otra función para que sea invocada más tarde. Es una forma de higher-order function: la función receptora decide cuándo, cómo y con qué argumentos llamar al callback. El callback en sí no tiene nada de especial — es una función normal.

```js
function cargarDatos(url, onSuccess, onError) {
  fetch(url)
    .then(r => r.json())
    .then(onSuccess)
    .catch(onError);
}

cargarDatos(
  "/api/users",
  datos => console.log("Datos recibidos:", datos),
  err  => console.error("Error:", err.message)
);
```

## Callbacks síncronos vs asíncronos

La distinción no está en el callback en sí, sino en **cuándo lo invoca la función receptora**:

```js
// Síncrono — el callback se ejecuta dentro del mismo stack, antes de que retorne forEach
[1, 2, 3].forEach(n => console.log(n)); // se ejecuta ahora, en este stack frame
console.log("esto corre DESPUÉS de forEach");

// Asíncrono — el callback se ejecuta en una iteración futura del Event Loop
setTimeout(() => console.log("callback"), 0);
console.log("esto corre ANTES que el callback");
```

| Característica | Callback síncrono | Callback asíncrono |
|---|---|---|
| Cuándo se ejecuta | Dentro del stack actual | En una iteración futura del Event Loop |
| Bloquea el hilo | Sí (mientras corre) | No (el hilo continúa) |
| Ejemplos comunes | forEach, map, filter, sort | setTimeout, fetch, eventos DOM |
| Predecibilidad del orden | Total | Depende del Event Loop |

## Anatomy: cómo se pasa e invoca un callback

```js
// La función receptora acepta un callback y lo llama cuando tiene datos
function procesarArchivo(ruta, callback) {
  const datos = leerArchivo(ruta);      // operación síncrona hipotética
  const resultado = transformar(datos);
  callback(resultado);                  // invoca el callback con el resultado
}

// El caller pasa la función que quiere que se ejecute
procesarArchivo("datos.csv", resultado => {
  console.log("Procesado:", resultado);
});
```

El caller no controla cuándo se llama a `callback` — eso lo decide `procesarArchivo`. Esto es la **inversión de control**.

## Inversión de control (Inversion of Control)

Cuando se usa un callback, el caller cede el control de cuándo y cómo se invoca su función:

```js
libreria.ejecutarOperacion(miCallback);
// Ahora es responsabilidad de "libreria" llamar miCallback:
// - ¿Cuántas veces? (¿una? ¿varias? ¿nunca?)
// - ¿Con qué argumentos?
// - ¿Síncrona o asincrónamente?
// - ¿En caso de error, la llama o no?
```

Esta pérdida de control es el origen de los problemas de composición de callbacks: el código externo (librería, API) decide el comportamiento que afecta directamente a tu lógica. Las Promises resuelven esto porque el caller recupera el control del flujo a través del objeto `Promise` devuelto.

## Callbacks con nombre vs anónimos

Los callbacks anónimos (arrow functions o function expressions inline) son convenientes pero dificultan la depuración — el stack trace muestra "anonymous" en lugar de un nombre útil:

```js
// Anónimo — stack trace confuso
fs.readFile("archivo.txt", "utf8", (err, datos) => {
  procesarDatos(datos);
});

// Con nombre — stack trace legible
function alLeerArchivo(err, datos) {
  if (err) return manejarError(err);
  procesarDatos(datos);
}

fs.readFile("archivo.txt", "utf8", alLeerArchivo);
```

Nombrar las funciones callback también facilita reutilizarlas y testearlas de forma independiente.

## Cómo funciona por dentro

JavaScript trata las funciones como valores de primera clase (`first-class functions`): pueden almacenarse en variables, pasarse como argumentos y devolverse como resultado de otras funciones. Un callback no es más que ese mecanismo aplicado a la comunicación asíncrona. Desde la perspectiva del motor, no hay diferencia entre `setTimeout(miFunc, 100)` y `[1,2,3].forEach(miFunc)` — en ambos casos `miFunc` es una referencia a función que se pasa como argumento.

> [!tip] Testear callbacks síncronamente
> Las funciones que aceptan callbacks síncronos son las más fáciles de testear — basta con pasar un callback que capture el resultado y hacer la aserción dentro o después. Para callbacks asíncronos, los frameworks de test (Jest, Mocha) ofrecen el parámetro `done` o soporte de Promises/async.

> [!warning] Callbacks no son necesariamente asíncronos
> Es un error común asumir que "callback = asíncrono". `Array.prototype.sort`, `Array.prototype.map`, `String.prototype.replace` con función, todos usan callbacks síncronos. Si el comportamiento (síncrono vs asíncrono) no está documentado, no se puede asumir.

## Notas relacionadas

- [[index|Callbacks — índice de la sección]]
- [[02 Callback Hell|Callback Hell — cuando los callbacks se anidan]]
- [[03 Error-First Pattern|Error-First Pattern — convención de Node.js]]
