---
title: Promise.prototype.finally — ejecutar siempre al settled
aliases:
  - .finally
  - promise finally
  - onFinally
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
order: 5
---

# Promise.prototype.finally

> [!definicion]
> `promise.finally(onFinally)` registra un callback que se ejecuta cuando la Promise se asienta, sin importar si fue `fulfilled` o `rejected`. A diferencia de `.then` y `.catch`, `onFinally` no recibe ningún argumento — no puede conocer el resultado. El valor o razón original pasa a través de `.finally` sin modificarse, salvo que `onFinally` lance o retorne una Promise rechazada.

```js
mostrarSpinner();

fetch('/api/datos')
  .then(res => res.json())
  .then(datos => renderizar(datos))
  .catch(err => mostrarError(err.message))
  .finally(() => ocultarSpinner()); // siempre se ejecuta, éxito o fallo
```

## Firma

```js
promise.finally(onFinally)
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `onFinally` | `Function` | Callback sin argumentos. Se ejecuta al settled. |

**Retorna:** una nueva Promise que adopta el valor/razón original, salvo excepción en `onFinally`.

## Transparencia del valor original

`.finally` no modifica el valor o razón que fluye por la cadena. El callback se ejecuta por sus efectos secundarios, no para transformar el resultado:

```js
Promise.resolve(42)
  .finally(() => console.log('settled'))  // → 'settled'
  .then(v => console.log(v));             // → 42 (valor original, no afectado)

Promise.reject(new Error('fallo'))
  .finally(() => console.log('settled'))  // → 'settled'
  .catch(err => console.error(err.message)); // → 'fallo' (razón original)
```

Comparado con `.then(fn, fn)`:

| Mecanismo | Recibe el valor | Puede transformar | Siempre se ejecuta |
|---|---|---|---|
| `.then(onF)` | Sí (solo fulfilled) | Sí | No |
| `.catch(onR)` | Sí (solo rejected) | Sí | No |
| `.finally(onF)` | No | Solo si lanza/rechaza | Sí |

## Cuándo `.finally` sí altera la cadena

Si `onFinally` lanza una excepción o retorna una Promise rechazada, la nueva Promise pasa a `rejected` con esa razón — ignorando el valor o razón original:

```js
Promise.resolve('datos')
  .finally(() => {
    throw new Error('fallo al cerrar conexión');
  })
  .then(v => console.log(v))              // no se ejecuta
  .catch(err => console.error(err.message)); // → 'fallo al cerrar conexión'
```

Esta propiedad es útil para propagar errores de cleanup (conexiones que no se pudieron cerrar, archivos que no se pudieron liberar).

## Receta: spinner de carga

El patrón más frecuente — garantizar que el spinner siempre se oculta independientemente del resultado:

```js
function cargarDatos(url) {
  mostrarSpinner();

  return fetch(url)
    .then(res => {
      if (!res.ok) throw new Error(`Error ${res.status}`);
      return res.json();
    })
    .finally(() => {
      ocultarSpinner(); // siempre: fetch exitoso, error de red, o error de parsing
    });
}

cargarDatos('/api/estadisticas')
  .then(datos => renderizarGrafica(datos))
  .catch(err => mostrarMensaje('No se pudo cargar: ' + err.message));
```

## Receta: cerrar conexiones y liberar recursos

```js
async function procesarConBD() {
  const conexion = await abrirConexion();

  return consultar(conexion, 'SELECT * FROM pedidos')
    .then(filas => transformar(filas))
    .finally(() => conexion.cerrar()); // siempre cierra, éxito o fallo
}
```

## Equivalencia conceptual

`.finally(fn)` es aproximadamente equivalente a:

```js
promise.then(
  valor => Promise.resolve(fn()).then(() => valor),
  razon => Promise.resolve(fn()).then(() => { throw razon; })
);
```

La diferencia es que la implementación nativa trata correctamente el caso en que `fn` devuelve una Promise rechazada.

> [!tip]
> `.finally` se encadena al final de la cadena principal pero antes del `.catch` solo si se quiere que el cleanup ocurra antes de manejar el error. Lo más común es colocarlo después del `.catch` para que el cleanup sea siempre lo último — independientemente de si el error fue manejado o no.

> [!warning]
> `onFinally` no recibe argumentos intencionalmente — si se necesita el valor o la razón dentro del callback de cleanup, hay que capturarlos en el scope externo o usar `.then(v => { cleanup(); return v; })` explícitamente. Intentar acceder al resultado desde `onFinally` siempre produce `undefined`.

## Notas relacionadas

- [[index|Promesas — índice de la sección]]
- [[03 then|.then — encadenar sobre fulfilled]]
- [[04 catch|.catch — capturar rechazos]]
- [[06 Chaining y Propagación de Errores|Chaining y Propagación de Errores]]
