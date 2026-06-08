---
title: Chaining y Propagación de Errores en Promesas
aliases:
  - promise chaining
  - encadenamiento de promesas
  - propagación de errores promise
tags:
  - javascript
  - api/concepto
  - asincrono
objeto: Promise
tipo: concepto
draft: false
---

# Chaining y Propagación de Errores

> [!definicion]
> El **encadenamiento de promesas** (promise chaining) es la técnica de conectar múltiples `.then` y `.catch` en secuencia, aprovechando que cada uno retorna una nueva Promise. Los errores se **propagan automáticamente** hacia adelante en la cadena, saltando todos los `.then` intermedios hasta encontrar el primer `.catch` (o handler `onRejected`) disponible.

```js
fetch('/api/usuario')
  .then(r => r.json())
  .then(usuario => fetch(`/api/posts/${usuario.id}`))
  .then(r => r.json())
  .then(posts => renderizarPosts(posts))
  .catch(err => manejarError(err)); // captura errores de cualquier eslabón
```

## Cómo se forma la cadena

Cada `.then` retorna una Promise nueva — no la misma. La nueva Promise adopta el valor retornado por `onFulfilled` o la razón lanzada. Esto crea una cadena de Promises independientes, no una sola Promise modificada:

```js
const p1 = Promise.resolve(1);
const p2 = p1.then(v => v + 1);
const p3 = p2.then(v => v * 10);

p3.then(v => console.log(v)); // → 20

// p1, p2, p3 son objetos distintos:
console.log(p1 === p2); // → false
```

## Propagación de errores

Un error en cualquier eslabón salta todos los `.then` intermedios hasta el próximo `.catch`:

```js
Promise.resolve('inicio')
  .then(() => {
    throw new Error('fallo en paso 2');
  })
  .then(v => {
    console.log('paso 3 — nunca se ejecuta');
    return v;
  })
  .then(v => {
    console.log('paso 4 — nunca se ejecuta');
    return v;
  })
  .catch(err => {
    console.error('capturado:', err.message); // → 'fallo en paso 2'
  });
```

El mecanismo: cuando `onFulfilled` lanza, la Promise retornada por ese `.then` pasa a `rejected`. El siguiente `.then` tiene `onFulfilled` pero no `onRejected`, así que pasa el rechazo. Y así sucesivamente hasta el `.catch`.

## `.then(fn, fn)` vs `.then(fn).catch(fn)`

La diferencia es cuándo se captura el error:

```js
// Patrón A — onRejected en el mismo .then
promesa.then(
  datos => procesar(datos),  // si lanza, el onRejected de abajo NO lo captura
  err   => recuperar(err)    // solo captura rechazos de `promesa`, no de `procesar`
);

// Patrón B — .catch separado
promesa
  .then(datos => procesar(datos)) // si lanza, el .catch sí lo captura
  .catch(err => recuperar(err));
```

El patrón A es útil cuando se quiere diferenciar "la promesa original falló" de "el handler falló". En la mayoría de casos, el patrón B es más robusto.

## Anti-patrón: anidar `.then` dentro de `.then`

El "pyramid of doom" reaparece si se anida en lugar de encadenar:

```js
// Incorrecto — vuelve al anidamiento
fetch('/api/usuario')
  .then(res => {
    res.json().then(usuario => {  // Promise anidada, no encadenada
      fetch(`/api/posts/${usuario.id}`).then(res2 => {
        res2.json().then(posts => renderizar(posts));
      });
    });
  });
```

La solución es retornar la Promise interna para que el chaining sea plano:

```js
// Correcto — retornar la Promise
fetch('/api/usuario')
  .then(res => res.json())               // retorna Promise<usuario>
  .then(usuario => fetch(`/api/posts/${usuario.id}`)) // retorna Promise<Response>
  .then(res => res.json())               // retorna Promise<posts>
  .then(posts => renderizar(posts));
```

Sin el `return`, la cadena externa continúa inmediatamente con `undefined` — la operación asíncrona interna queda "flotante" y sin manejo de errores.

## Manejo de errores por etapas

Es posible colocar `.catch` intermedios para recuperarse de ciertos errores y continuar la cadena:

```js
obtenerUsuario(id)
  .catch(err => {
    console.warn('Usuario no encontrado, usando anónimo');
    return { nombre: 'Anónimo', id: null }; // recupera con valor por defecto
  })
  .then(usuario => cargarPreferencias(usuario))
  .catch(err => {
    console.warn('Sin preferencias, usando defaults');
    return preferenciasDefecto;
  })
  .then(prefs => inicializarApp(prefs))
  .catch(err => {
    console.error('Error fatal:', err.message);
    mostrarPantallaDeError();
  });
```

Cada `.catch` intermedio "atrapa" el error del eslabón anterior y puede decidir recuperarse (retornando un valor) o relanzar (con `throw`).

## Receta: cadena completa con transformación y manejo de errores

```js
function cargarPerfilCompleto(usuarioId) {
  return fetch(`/api/usuarios/${usuarioId}`)
    .then(res => {
      if (!res.ok) throw new Error(`Usuario ${usuarioId} no existe (${res.status})`);
      return res.json();
    })
    .then(usuario => Promise.all([
      Promise.resolve(usuario),
      fetch(`/api/usuarios/${usuario.id}/avatar`).then(r => r.blob())
    ]))
    .then(([usuario, avatarBlob]) => ({
      ...usuario,
      avatarUrl: URL.createObjectURL(avatarBlob)
    }))
    .catch(err => {
      console.error('cargarPerfilCompleto falló:', err.message);
      throw err; // relanza para que el caller también pueda manejarlo
    });
}
```

> [!tip]
> Relanzar el error en un `.catch` intermedio (`throw err`) permite que el error sea visible tanto en el punto donde ocurrió (log interno) como en el caller que consume la Promise. Sin relanzar, el `.catch` absorbe el error y la Promise retornada queda `fulfilled` con `undefined`.

> [!warning]
> Crear una cadena sin `.catch` final en producción es una fuente de errores silenciosos. En ambientes Node.js modernos, los rechazos sin manejar terminan el proceso. En el browser, disparan `unhandledrejection`. Siempre añadir un `.catch` final aunque sea solo para registrar el error.

## Notas relacionadas

- [[index|Promesas — índice de la sección]]
- [[03 then|.then — comportamiento del valor de retorno]]
- [[04 catch|.catch — recuperación vs propagación]]
- [[07 Promise.all|Promise.all — operaciones paralelas en la cadena]]
