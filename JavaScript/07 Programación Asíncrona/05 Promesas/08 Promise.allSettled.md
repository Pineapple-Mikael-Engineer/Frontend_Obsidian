---
title: Promise.allSettled — esperar todas sin fail-fast
aliases:
  - Promise.allSettled
  - promise allSettled
  - allSettled
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

# Promise.allSettled

> [!definicion]
> `Promise.allSettled(iterable)` retorna una Promise que se cumple cuando **todas** las Promises del iterable se asientan — sin importar si terminaron en `fulfilled` o `rejected`. La Promise retornada **nunca se rechaza**. El resultado es un array de objetos descriptores con la forma `{ status: "fulfilled", value }` o `{ status: "rejected", reason }`.

```js
const resultados = await Promise.allSettled([
  fetch('/api/servicio-a').then(r => r.json()),
  fetch('/api/servicio-b').then(r => r.json()),
  fetch('/api/servicio-c').then(r => r.json())
]);

for (const r of resultados) {
  if (r.status === 'fulfilled') {
    console.log('Éxito:', r.value);
  } else {
    console.error('Fallo:', r.reason.message);
  }
}
```

## Firma

```js
Promise.allSettled(iterable)
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `iterable` | `Iterable<Promise \| valor>` | Array u otro iterable de Promises o valores. |

**Retorna:** una Promise que siempre se cumple con un array de objetos descriptores.

## Forma de los objetos de resultado

Cada elemento del array de resultado corresponde a la Promise en la misma posición del iterable:

```js
// Promise fulfilled
{ status: "fulfilled", value: <valor con que se cumplió> }

// Promise rejected
{ status: "rejected", reason: <razón del rechazo> }
```

```js
const resultados = await Promise.allSettled([
  Promise.resolve(1),
  Promise.reject(new Error('fallo')),
  Promise.resolve(3)
]);

console.log(resultados);
// [
//   { status: 'fulfilled', value: 1 },
//   { status: 'rejected',  reason: Error: fallo },
//   { status: 'fulfilled', value: 3 }
// ]
```

## Comparativa con Promise.all

| Característica | Promise.all | Promise.allSettled |
|---|---|---|
| Se cumple cuando | Todas se cumplen | Todas se asientan |
| Se rechaza cuando | Cualquiera se rechaza | Nunca |
| Tipo de resultado | Array de valores | Array de `{status, value/reason}` |
| Comportamiento ante fallo | Fail-fast — aborta | Espera a todas |
| Cuándo usar | Todas son necesarias | Reporte de éxitos y fallos |

## Cuándo usar

`Promise.allSettled` se justifica cuando:

- Se necesita el resultado de **todas** las operaciones aunque algunas fallen.
- Se va a reportar o registrar qué falló y qué no, sin interrumpir el flujo.
- Las operaciones son independientes y un fallo parcial es un resultado válido (no un error fatal).

Ejemplos típicos: enviar notificaciones a múltiples destinatarios, sincronizar datos a múltiples endpoints, batch de operaciones de base de datos.

## Receta: operación bulk con reporte de éxitos y fallos

```js
async function enviarNotificaciones(destinatarios, mensaje) {
  const resultados = await Promise.allSettled(
    destinatarios.map(dest => enviarEmail(dest, mensaje))
  );

  const exitosos = resultados
    .filter(r => r.status === 'fulfilled')
    .map((_, i) => destinatarios[i]);

  const fallidos = resultados
    .filter(r => r.status === 'rejected')
    .map((r, i) => ({ destinatario: destinatarios[i], error: r.reason.message }));

  console.log(`Enviados: ${exitosos.length}/${destinatarios.length}`);
  if (fallidos.length) {
    console.warn('Fallaron:', fallidos);
  }

  return { exitosos, fallidos };
}
```

## Receta: separar éxitos y fallos en una línea

```js
const resultados = await Promise.allSettled(promesas);

const valores  = resultados.filter(r => r.status === 'fulfilled').map(r => r.value);
const errores  = resultados.filter(r => r.status === 'rejected').map(r => r.reason);
```

## Disponibilidad

`Promise.allSettled` es parte de ES2020. Disponible en todos los navegadores modernos y Node.js ≥ 12.9. No requiere polyfill en ningún entorno de producción actual.

> [!tip]
> Para obtener solo los valores de las Promises exitosas e ignorar los fallos silenciosamente, `Promise.allSettled` combinado con `.filter` es más claro que `Promise.all` con `.catch(() => null)` en cada Promise. El segundo patrón pierde información sobre cuáles fallaron y por qué.

> [!warning]
> Dado que `Promise.allSettled` nunca rechaza, un `.catch` al final de su cadena solo capturará errores lanzados en los handlers `.then` posteriores — no en las Promises del iterable. No confundir: la Promise de `allSettled` siempre se cumple, pero el código que procesa sus resultados puede fallar.

## Notas relacionadas

- [[index|Promesas — índice de la sección]]
- [[07 Promise.all|Promise.all — fail-fast, todas necesarias]]
- [[09 Promise.race|Promise.race — la primera en asentarse]]
- [[10 Promise.any|Promise.any — la primera fulfilled]]
