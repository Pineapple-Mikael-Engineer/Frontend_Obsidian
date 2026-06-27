---
title: try / catch / finally — manejo de errores síncronos
aliases:
  - try catch
  - try catch finally js
  - manejo de errores js
tags:
  - javascript
  - api/concepto
  - errores
objeto: "-"
tipo: concepto
retorna: "-"
muta: false
asincrono: false
draft: false
order: 2
---

# try / catch / finally

> [!definicion]
> `try/catch/finally` es la construcción de manejo de errores síncronos de JavaScript. El bloque `try` delimita el código que puede fallar; `catch` recibe el valor lanzado; `finally` se ejecuta siempre, con o sin error, incluso si alguno de los bloques anteriores tiene `return`.

```js
try {
  const data = JSON.parse(input);
  procesarDatos(data);
} catch (err) {
  console.error(err.name, err.message);
} finally {
  limpiarEstadoTemporal();
}
```

## Estructura completa

```js
try {
  // código que puede lanzar con throw o por error del motor
} catch (err) {
  // err: el valor lanzado (por convención, siempre un objeto Error)
  // Este bloque solo corre si try lanzó
} finally {
  // Siempre corre: con error, sin error, con return en try/catch
}
```

Las tres cláusulas son opcionales de forma combinada: `try/catch`, `try/finally` y `try/catch/finally` son las tres formas válidas. `try` solo (sin `catch` ni `finally`) es un error de sintaxis.

## Alcance de captura

`try/catch` captura solo errores **síncronos** que se propagan dentro del bloque `try`. Los errores asíncronos (callbacks de `setTimeout`, event listeners, Promises sin `await`) **no** se capturan desde un `try` externo.

```js
// Captura: error síncrono
try {
  null.foo; // TypeError síncrono
} catch (err) { console.log("capturado"); }

// No captura: callback asíncrono
try {
  setTimeout(() => { null.foo; }, 0); // el error ocurre fuera del try
} catch (err) { console.log("esto no corre"); }

// Sí captura con await:
try {
  const res = await fetch("/api/datos");
} catch (err) { console.log("capturado"); }
```

## Notas de la sección

- [[02 try - catch - finally/01 try y catch | try y catch]] — captura, discriminación de tipo, sincronía vs asincronía
- [[02 try - catch - finally/02 finally | finally]] — semántica de ejecución garantizada, return anulado, limpieza de recursos

## Notas relacionadas

- [[01 Tipos de Errores/index | Tipos de Errores]]
- [[03 throw/index | throw y errores personalizados]]
