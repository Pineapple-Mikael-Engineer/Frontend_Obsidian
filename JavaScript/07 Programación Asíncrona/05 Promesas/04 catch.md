---
title: Promise.prototype.catch — capturar rechazos en la cadena
aliases:
  - .catch
  - promise catch
  - onRejected
tags:
  - javascript
  - api/metodo
  - asincrono
objeto: Promise
tipo: metodo
retorna: Promise
muta: false
asincrono: true
draft: false
---

# Promise.prototype.catch

> [!definicion]
> `promise.catch(onRejected)` es azúcar sintáctica para `promise.then(undefined, onRejected)`. Registra un handler que se invoca cuando la Promise pasa a `rejected`, capturando tanto el rechazo de la promesa original como cualquier error lanzado en un `.then` anterior de la cadena. Retorna una nueva Promise.

```js
fetch('/api/datos')
  .then(res => {
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    return res.json();
  })
  .then(datos => procesarDatos(datos))
  .catch(err => {
    // captura errores de fetch, del primer .then y del segundo .then
    console.error('Falló la operación:', err.message);
  });
```

## Firma

```js
promise.catch(onRejected)
// equivale a:
promise.then(undefined, onRejected)
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `onRejected` | `Function` | Callback invocado con la razón de rechazo. Recibe el error o valor con que se rechazó la Promise. |

**Retorna:** una nueva Promise. Su estado depende de lo que haga `onRejected`.

## Comportamiento del valor de retorno

Si `onRejected` no lanza y no retorna una Promise rechazada, la nueva Promise pasa a `fulfilled` — la cadena se recupera del error y continúa:

```js
Promise.reject(new Error('red caída'))
  .catch(err => {
    console.warn('Reintentando con cache:', err.message);
    return obtenerDeCache(); // se "recupera" — la cadena continúa fulfilled
  })
  .then(datos => renderizar(datos)) // se ejecuta con los datos del cache
  .catch(err => console.error('Sin cache tampoco:', err.message));
```

| El `onRejected`... | La nueva Promise... |
|---|---|
| Retorna un valor | Pasa a `fulfilled` con ese valor |
| Retorna una Promise fulfilled | Adopta ese estado fulfilled |
| Lanza una excepción | Pasa a `rejected` con esa excepción |
| Retorna una Promise rejected | Pasa a `rejected` con esa razón |

## Propagación de errores sin `.catch`

Si una Promise rechazada no tiene handler, el motor dispara el evento `unhandledrejection`. En el navegador:

```js
window.addEventListener('unhandledrejection', event => {
  console.error('Promise sin handler:', event.reason);
  event.preventDefault(); // evita el log de error por defecto en el browser
});
```

En Node.js es el evento `process.on('unhandledRejection', ...)`. En ambos entornos, los rechazos sin handler en producción suelen cerrar el proceso o generar warnings en consola.

## Diferencia entre `.then(fn, fn)` y `.then(fn).catch(fn)`

Esta distinción importa cuando el `onFulfilled` puede lanzar:

```js
// Patrón A: .then(onFulfilled, onRejected)
promesa.then(
  datos => {
    throw new Error('error en onFulfilled'); // NO lo captura el onRejected del mismo .then
  },
  err => console.error('solo captura rechazos de promesa, no del onFulfilled')
);

// Patrón B: .then(onFulfilled).catch(onRejected)
promesa
  .then(datos => {
    throw new Error('error en onFulfilled'); // SÍ lo captura el .catch
  })
  .catch(err => console.error('captura todo:', err.message));
```

El patrón B (`.then(...).catch(...)`) es más robusto y el preferido salvo que se quiera diferenciar explícitamente entre "la promesa falló" y "el handler falló".

## Receta: manejo de errores de red con lógica de retry

```js
function fetchConRetry(url, intentos = 3) {
  return fetch(url).catch(err => {
    if (intentos <= 1) throw err;
    console.warn(`Reintentando (${intentos - 1} intentos restantes)...`);
    return fetchConRetry(url, intentos - 1);
  });
}

fetchConRetry('/api/critico')
  .then(res => res.json())
  .then(datos => renderizar(datos))
  .catch(err => mostrarMensajeDeError('No se pudo conectar al servidor'));
```

## Receta: un único `.catch` al final de la cadena

El patrón más limpio para la UI: un único punto de manejo de errores al final de la cadena, independientemente de qué paso falló:

```js
cargarUsuario(id)
  .then(usuario => cargarPermisos(usuario.id))
  .then(permisos => cargarDashboard(permisos))
  .then(datos => renderizarDashboard(datos))
  .catch(err => {
    ocultarSpinner();
    mostrarBanner(`Error: ${err.message}`);
    registrarEnAnalytics('error_carga_dashboard', err);
  });
```

> [!tip]
> Para reintentar solo ciertos tipos de error, comprobar `err instanceof` dentro del `.catch`. Si el error no es recuperable, relanzarlo con `throw err` para que siga propagándose:
> ```js
> .catch(err => {
>   if (err instanceof NetworkError) return reintentar();
>   throw err; // otros errores se propagan
> })
> ```

> [!warning]
> Un `.catch` al final de la cadena no captura errores que ocurran dentro de callbacks asíncronos no encadenados a la Promise. Por ejemplo, un error dentro de un `setTimeout` dentro de un `.then` no se captura — ese `setTimeout` crea una nueva macrotask fuera de la cadena de Promises.

## Notas relacionadas

- [[index|Promesas — índice de la sección]]
- [[03 then|.then — diferencia con .then(fn, fn)]]
- [[05 finally|.finally — ejecutar siempre tras el settle]]
- [[06 Chaining y Propagación de Errores|Chaining y Propagación de Errores]]
