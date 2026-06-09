---
title: try y catch — captura de errores síncronos
aliases:
  - try catch js
  - catch binding
  - optional catch binding
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
---

# try y catch

> [!definicion]
> `try` delimita el bloque de código que puede lanzar; `catch` captura el valor lanzado y permite manejarlo sin que el error se propague. En cuanto se lanza cualquier valor dentro de `try`, la ejecución salta inmediatamente al bloque `catch`, saltando el resto de `try`.

```js
try {
  const obj = JSON.parse(entrada);  // puede lanzar SyntaxError
  procesarObjeto(obj);              // puede lanzar TypeError
} catch (err) {
  // err recibe el error lanzado
  console.error(err.name, err.message);
}
// La ejecución continúa aquí tanto si hubo error como si no
```

## El parámetro de catch

`catch (err)` recibe cualquier valor lanzado, no solo instancias de `Error`. En JS se puede lanzar un string, número u objeto plano con `throw`. Por convención, siempre se lanza un objeto `Error` o subclase para que `err.name`, `err.message` y `err.stack` estén disponibles.

### Optional catch binding (ES2019)

Desde ES2019, el parámetro de `catch` es opcional. Si no se necesita el valor del error, se omite:

```js
try {
  operacionQuePuedeFallar();
} catch {
  // no se necesita el error, solo recuperar
  usarValorPorDefecto();
}
```

Útil cuando el bloque catch ejecuta una recuperación fija independientemente del error.

## Discriminar el tipo de error

Dos estrategias para manejar tipos distintos en el mismo catch:

```js
// Por instanceof (preferida: sobrevive a subclases)
try {
  // ...
} catch (err) {
  if (err instanceof SyntaxError) {
    console.error("JSON inválido:", err.message);
  } else if (err instanceof TypeError) {
    console.error("Tipo inesperado:", err.message);
  } else {
    throw err; // relanzar lo que no se maneja explícitamente
  }
}

// Por err.name (útil cuando el error cruza iframes o workers)
try {
  // ...
} catch (err) {
  if (err.name === "ValidationError") {
    mostrarErrorForm(err.message);
  } else {
    throw err;
  }
}
```

`instanceof` falla si el error cruza contextos (iframes, workers) porque cada contexto tiene su propio `Error.prototype`. En esos casos, comparar `err.name` como string es más robusto.

## Qué captura y qué no captura

| Escenario | Capturado por try/catch |
|-----------|------------------------|
| Error síncrono dentro de try | Sí |
| `throw` explícito síncrono | Sí |
| Callback de `setTimeout` / `setInterval` | No |
| Event listener (`addEventListener`) | No |
| Promise rechazada sin `await` | No |
| Promise rechazada con `await` dentro de try | Sí |
| `async/await` dentro de try | Sí |

```js
// Asíncrono con await: sí captura
async function cargar(url) {
  try {
    const res = await fetch(url);
    const data = await res.json();
    return data;
  } catch (err) {
    // captura errores de red (TypeError de fetch) y JSON inválido (SyntaxError)
    console.error("Error al cargar:", err.message);
    return null;
  }
}
```

## Cómo funciona por dentro

Cuando el motor evalúa `try { ... }`, instala un **registro de control** (completion record de tipo `throw`) en el contexto de ejecución. Si cualquier operación dentro del bloque lanza, el motor busca el handler más cercano en la cadena de call stack. Si encuentra un `catch` activo, transfiere el control ahí con el valor lanzado como binding del parámetro. Si no hay ningún `catch`, el error escala hasta el host (browser o Node), que lo reporta como "Uncaught Error".

## Anidamiento de try/catch

`try` puede anidarse dentro de `catch` para manejar errores secundarios sin contaminar el handler externo:

```js
try {
  const data = obtenerDatos();
} catch (err) {
  try {
    logearError(err); // esto también puede fallar
  } catch (logErr) {
    console.error("No se pudo loguear:", logErr);
  }
  mostrarMensajeGenerico();
}
```

> [!tip]
> Relanzar errores no manejados (`throw err` al final del catch) es la práctica más importante. Un catch que silencia todo convierte los bugs en comportamientos invisibles. Solo capturar los errores que el código sabe manejar concretamente.

> [!warning]
> `try/catch` tiene un costo de rendimiento marginal en la mayoría de engines: V8 no optimiza funciones que contienen `try/catch` en el mismo nivel del código caliente. Para rutas críticas de rendimiento, mover el `try/catch` a una función wrapper separada y llamarla desde la ruta optimizada.

## Notas relacionadas

- [[02 try - catch - finally/index | try / catch / finally]]
- [[02 try - catch - finally/02 finally | finally]]
- [[01 Tipos de Errores/index | Tipos de Errores]]
- [[03 throw/index | throw y errores personalizados]]
