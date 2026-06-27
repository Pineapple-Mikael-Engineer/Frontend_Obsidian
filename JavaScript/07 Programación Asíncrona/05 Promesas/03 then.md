---
title: Promise.prototype.then — encadenar handlers sobre una Promise
aliases:
  - .then
  - promise then
  - onFulfilled
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
order: 3
---

# Promise.prototype.then

> [!definicion]
> `promise.then(onFulfilled, onRejected?)` registra hasta dos callbacks: `onFulfilled` se invoca cuando la promesa pasa a `fulfilled`; `onRejected` (opcional) cuando pasa a `rejected`. Siempre retorna **una nueva Promise** cuyo valor depende del valor de retorno de los callbacks — lo que permite encadenar operaciones asíncronas de forma plana.

```js
fetch('/api/usuario/5')
  .then(res => res.json())          // onFulfilled transforma el Response en objeto
  .then(usuario => {
    console.log(usuario.nombre);    // → 'Ana'
    return usuario.id;
  })
  .then(id => console.log('id:', id)); // → 'id: 5'
```

## Firma

```js
promise.then(onFulfilled)
promise.then(onFulfilled, onRejected)
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `onFulfilled` | `Function \| null` | Callback invocado con el valor fulfilled. Si no es función, el valor pasa al siguiente `.then`. |
| `onRejected` | `Function \| null` | Callback invocado con la razón de rechazo. Si no es función, el rechazo se propaga. |

**Retorna:** una nueva Promise. El estado de esa Promise depende de lo que retorne el callback activo.

## Qué retorna la nueva Promise según el callback

| El callback... | La nueva Promise... |
|---|---|
| Retorna un valor primitivo u objeto | Pasa a `fulfilled` con ese valor |
| Retorna una Promise o thenable | Adopta el estado de esa Promise (flattening) |
| Lanza una excepción | Pasa a `rejected` con esa excepción |
| No está definido (o no es función) | Pasa el valor/razón original al siguiente eslabón |

## Asincronía garantizada de los callbacks

Los callbacks de `.then` se ejecutan **siempre de forma asíncrona** como microtasks, incluso si la Promise ya está settled en el momento en que se llama a `.then`:

```js
const p = Promise.resolve('valor');

p.then(v => console.log('then:', v));
console.log('síncrono');

// → síncrono
// → then: valor
```

Esto garantiza un comportamiento predecible: el callback nunca se ejecuta dentro del stack de la llamada a `.then`.

## Thenable flattening (aplanado de Promises)

Si `onFulfilled` devuelve una Promise, la nueva Promise no contiene a la Promise devuelta — adopta su estado. Las Promises no se anidan:

```js
fetch('/api/usuario/1')
  .then(res => res.json())
  .then(usuario => fetch(`/api/posts/${usuario.id}`)) // devuelve Promise — no se anida
  .then(res => res.json())                            // recibe el Response de la segunda fetch
  .then(posts => console.log(posts.length));
```

Si `.then` anidara Promises en lugar de aplanarlas, el resultado sería `Promise<Promise<Response>>` — inutilizable.

## Propagación cuando el callback no está definido

Si `onFulfilled` no es una función, el valor fulfilled se transmite al siguiente `.then` sin modificación. Equivale a `.then(v => v)`:

```js
Promise.resolve(10)
  .then(null)        // onFulfilled no es función — valor pasa
  .then(v => console.log(v)); // → 10
```

Esto es lo que permite que `.catch` capture errores lanzados varias posiciones atrás en la cadena.

## Receta: transformación encadenada

Secuencia de transformaciones síncronas y asíncronas combinadas:

```js
obtenerPedido(orderId)
  .then(pedido => {
    if (!pedido) throw new Error('Pedido no encontrado');
    return pedido;
  })
  .then(pedido => calcularImpuestos(pedido))  // síncrono — retorna valor
  .then(pedidoConImpuestos => guardarEnBD(pedidoConImpuestos)) // asíncrono — retorna Promise
  .then(guardado => enviarEmail(guardado.email))
  .then(() => console.log('Pedido procesado'))
  .catch(err => console.error('Error en flujo:', err.message));
```

> [!tip]
> Cuando el segundo argumento de `.then(onFulfilled, onRejected)` maneja el error, ese handler no captura los errores lanzados por el propio `onFulfilled` del mismo `.then`. Para capturar también esos errores, encadenar un `.catch` separado: `.then(fn).catch(fn)`.

> [!warning]
> Olvidar el `return` dentro de un `.then` que realiza una operación asíncrona es un error frecuente. Sin `return`, la cadena continúa inmediatamente con `undefined` en lugar de esperar el resultado de la operación interna:
> ```js
> // Incorrecto — continúa sin esperar guardar()
> .then(datos => { guardar(datos); })
>
> // Correcto
> .then(datos => guardar(datos))
> ```

## Notas relacionadas

- [[index|Promesas — índice de la sección]]
- [[04 catch|.catch — capturar rechazos de la cadena]]
- [[06 Chaining y Propagación de Errores|Chaining y Propagación de Errores]]
- [[01 Estados (pending, fulfilled, rejected)|Estados de una Promise]]
