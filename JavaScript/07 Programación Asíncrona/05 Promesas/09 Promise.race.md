---
title: Promise.race — adoptar el estado de la primera en asentarse
aliases:
  - Promise.race
  - promise race
  - race condition promise
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

# Promise.race

> [!definicion]
> `Promise.race(iterable)` retorna una Promise que adopta el estado de la **primera** Promise del iterable que se asiente — ya sea `fulfilled` o `rejected` — ignorando el resultado de las demás. Si la primera en asentarse está fulfilled, la Promise retornada se cumple con su valor. Si está rejected, se rechaza con su razón.

```js
const respuesta = await Promise.race([
  fetch('/api/servidor-principal').then(r => r.json()),
  fetch('/api/servidor-espejo').then(r => r.json())
]);

console.log(respuesta); // datos del servidor que respondió primero
```

## Firma

```js
Promise.race(iterable)
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `iterable` | `Iterable<Promise \| valor>` | Array u otro iterable de Promises. Un valor no-Promise se comporta como ya resuelto. |

**Retorna:** una Promise que adopta el estado (fulfilled o rejected) de la primera Promise que se asiente.

## Comportamiento

| Caso | Resultado |
|---|---|
| La primera en asentarse es fulfilled | La Promise retornada pasa a `fulfilled` con ese valor |
| La primera en asentarse es rejected | La Promise retornada pasa a `rejected` con esa razón |
| Iterable vacío | La Promise retornada queda `pending` para siempre |
| Un valor no-Promise en el iterable | Se resuelve inmediatamente — siempre "gana" la carrera |

## Iterable vacío — pending permanente

```js
Promise.race([]).then(v => console.log('nunca')); // pending forever
```

Nadie se asienta, así que la Promise de `race` tampoco lo hace nunca. Pasar un iterable vacío a `Promise.race` es un bug en la mayoría de contextos.

## Receta: timeout de operación asíncrona

El caso de uso más frecuente: limitar el tiempo que se espera por una operación.

```js
function conTimeout(promise, ms) {
  const timeout = new Promise((_, reject) =>
    setTimeout(() => reject(new Error(`Timeout de ${ms}ms`)), ms)
  );
  return Promise.race([promise, timeout]);
}

conTimeout(fetch('/api/lento'), 3000)
  .then(res => res.json())
  .then(datos => renderizar(datos))
  .catch(err => {
    if (err.message.startsWith('Timeout')) {
      mostrarMensaje('El servidor tardó demasiado. Inténtalo de nuevo.');
    } else {
      mostrarMensaje('Error de red: ' + err.message);
    }
  });
```

La operación original sigue ejecutándose tras el timeout (no se puede cancelar en JS estándar), pero su resultado se ignora.

## Receta: caché local vs servidor

Devolver el resultado más rápido entre la caché y una petición de red:

```js
function obtenerConCache(clave, fetchFn) {
  const desdeCache = leerCache(clave).then(datos => {
    if (!datos) return new Promise(() => {}); // pending si no hay caché — cede el paso al fetch
    return datos;
  });

  const desdeRed = fetchFn().then(datos => {
    guardarEnCache(clave, datos); // actualiza la caché en segundo plano
    return datos;
  });

  return Promise.race([desdeCache, desdeRed]);
}

const usuario = await obtenerConCache('usuario-5', () =>
  fetch('/api/usuarios/5').then(r => r.json())
);
```

## Receta: CDN race — múltiples servidores

```js
const servidores = [
  'https://cdn1.ejemplo.com/app.js',
  'https://cdn2.ejemplo.com/app.js',
  'https://cdn3.ejemplo.com/app.js'
];

const script = await Promise.race(
  servidores.map(url => fetch(url).then(r => {
    if (!r.ok) throw new Error(`${url} devolvió ${r.status}`);
    return r.text();
  }))
);
```

> [!tip]
> Para timeouts en `async/await`, `Promise.race` es la herramienta estándar. `AbortController` puede cancelar la petición subyacente (fetch), pero `Promise.race` maneja la lógica de cuál resultado importa. Combinarlos da timeouts con limpieza real:
> ```js
> const controller = new AbortController();
> const timeout = setTimeout(() => controller.abort(), 5000);
> try {
>   const res = await fetch(url, { signal: controller.signal });
>   clearTimeout(timeout);
>   return await res.json();
> } catch (err) { /* AbortError o error de red */ }
> ```

> [!warning]
> Si la primera Promise en asentarse es un **rechazo**, `Promise.race` se rechaza aunque las demás vayan a cumplirse. Esto puede ser inesperado en patrones de CDN fallback donde un servidor responde con error antes que otro con éxito. Para ese caso, usar [[10 Promise.any|Promise.any]], que ignora los rechazos y espera el primer fulfilled.

## Notas relacionadas

- [[index|Promesas — índice de la sección]]
- [[10 Promise.any|Promise.any — primera fulfilled, ignora rechazos]]
- [[07 Promise.all|Promise.all — esperar todas]]
- [[02 Crear Promesa (resolve, reject)|Crear una Promise — patrón de timeout con resolve/reject]]
