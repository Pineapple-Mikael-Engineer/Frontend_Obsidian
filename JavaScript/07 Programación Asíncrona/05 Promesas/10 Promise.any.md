---
title: Promise.any — primera Promise fulfilled, ignorando rechazos
aliases:
  - Promise.any
  - promise any
  - AggregateError
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

# Promise.any

> [!definicion]
> `Promise.any(iterable)` retorna una Promise que se cumple con el valor de la **primera** Promise del iterable que pase a `fulfilled`, ignorando los rechazos intermedios. Solo se rechaza si **todas** las Promises se rechazan, en cuyo caso la razón es un `AggregateError` que contiene todas las razones de rechazo en orden.

```js
const datos = await Promise.any([
  fetch('https://api-primaria.ejemplo.com/datos').then(r => r.json()),
  fetch('https://api-espejo.ejemplo.com/datos').then(r => r.json()),
  fetch('https://api-backup.ejemplo.com/datos').then(r => r.json())
]);

// datos provienen del endpoint que respondió primero con éxito
console.log(datos);
```

## Firma

```js
Promise.any(iterable)
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `iterable` | `Iterable<Promise \| valor>` | Array u otro iterable de Promises. Un valor no-Promise se comporta como ya resuelto y siempre "gana". |

**Retorna:** una Promise que se cumple con el valor de la primera fulfilled, o se rechaza con `AggregateError` si todas fallan.

## Comportamiento

| Caso | Resultado |
|---|---|
| La primera en cumplirse | La Promise retornada pasa a `fulfilled` con ese valor |
| Todas se rechazan | La Promise retornada pasa a `rejected` con `AggregateError` |
| Iterable vacío | La Promise retornada pasa a `rejected` con `AggregateError` (sin errores) |
| Un valor no-Promise | Se comporta como ya fulfilled — siempre gana |

## AggregateError

Cuando todas las Promises se rechazan, el rechazo es un `AggregateError` — un tipo de error diseñado para contener múltiples razones:

```js
Promise.any([
  Promise.reject(new Error('fallo A')),
  Promise.reject(new Error('fallo B')),
  Promise.reject(new Error('fallo C'))
])
.catch(err => {
  console.log(err instanceof AggregateError);  // → true
  console.log(err.message);                    // → 'All promises were rejected'
  console.log(err.errors);                     // → [Error: fallo A, Error: fallo B, Error: fallo C]
  console.log(err.errors[0].message);          // → 'fallo A'
});
```

`err.errors` es un array con todas las razones de rechazo, en el mismo orden del iterable (no en el orden en que se rechazaron).

## Promise.race vs Promise.any

La distinción clave:

| Característica | Promise.race | Promise.any |
|---|---|---|
| Se cumple cuando | La primera en asentarse es fulfilled | La primera fulfilled, ignora rechazos |
| Se rechaza cuando | La primera en asentarse es rejected | Todas se rechazan |
| Sensible a rechazos | Sí — el primer rechazo cierra la carrera | No — ignora rechazos hasta que todos fallan |
| Error en rechazo total | Error de la primera que rechazó | `AggregateError` con todas las razones |
| Caso típico | Timeout, carrera pura | Fallback entre fuentes, CDN |

## Tabla comparativa de métodos estáticos

| Método | Se cumple cuando | Se rechaza cuando | Resultado fulfilled | Resultado rejected |
|---|---|---|---|---|
| `Promise.all` | Todas fulfilled | Primera rejected | Array de valores (orden iterable) | Primera razón |
| `Promise.allSettled` | Todas settled (nunca rechaza) | Nunca | Array de `{status, value/reason}` | — |
| `Promise.race` | Primera settled es fulfilled | Primera settled es rejected | Valor de la primera | Razón de la primera |
| `Promise.any` | Primera fulfilled | Todas rejected | Valor de la primera fulfilled | `AggregateError` con todas |

## Receta: fetch con fallback a múltiples mirrors

```js
async function obtenerRecurso(ruta) {
  const mirrors = [
    `https://cdn1.ejemplo.com${ruta}`,
    `https://cdn2.ejemplo.com${ruta}`,
    `https://cdn3.ejemplo.com${ruta}`
  ];

  try {
    const respuesta = await Promise.any(
      mirrors.map(url => fetch(url).then(r => {
        if (!r.ok) throw new Error(`${url}: HTTP ${r.status}`);
        return r;
      }))
    );
    return respuesta;
  } catch (err) {
    // err es AggregateError — todos los mirrors fallaron
    console.error('Todos los mirrors fallaron:');
    err.errors.forEach(e => console.error(' -', e.message));
    throw new Error(`Recurso ${ruta} no disponible en ningún mirror`);
  }
}
```

## Receta: autenticación con múltiples proveedores, el primero que responda

```js
const tokenValido = await Promise.any([
  verificarConProveedorA(token),
  verificarConProveedorB(token),
  verificarEnCache(token)
]);
// Se usa el primer proveedor que confirme el token — los demás se ignoran
```

## Disponibilidad

`Promise.any` es parte de ES2021. Disponible en todos los navegadores modernos y Node.js ≥ 15. No requiere polyfill en entornos de producción actuales.

> [!tip]
> `Promise.any` es la elección correcta cuando se tienen múltiples fuentes intercambiables y basta con que una funcione. `Promise.race` es la elección cuando importa la velocidad pero también se quiere saber si todas fallaron rápidamente (un fallo temprano es un resultado válido).

> [!warning]
> Si el iterable está vacío, `Promise.any` se rechaza inmediatamente con un `AggregateError` sin errores (`err.errors = []`). Verificar que el iterable tiene al menos un elemento antes de llamar a `Promise.any` si la lista puede ser dinámica — un iterable vacío es un comportamiento probablemente inesperado.

## Notas relacionadas

- [[index|Promesas — índice de la sección]]
- [[09 Promise.race|Promise.race — primera en asentarse, fulfilled o rejected]]
- [[07 Promise.all|Promise.all — todas se necesitan]]
- [[08 Promise.allSettled|Promise.allSettled — reporte de todas]]
