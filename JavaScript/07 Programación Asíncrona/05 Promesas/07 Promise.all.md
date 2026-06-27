---
title: Promise.all — esperar que todas las promesas se cumplan
aliases:
  - Promise.all
  - promise all
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
order: 7
---

# Promise.all

> [!definicion]
> `Promise.all(iterable)` retorna una Promise que se cumple cuando **todas** las Promises del iterable se cumplen, con un array de sus valores en el mismo orden del iterable. Si **cualquiera** se rechaza (fail-fast), la Promise retornada se rechaza inmediatamente con esa razón — ignorando el estado de las demás.

```js
const [usuario, perfil, prefs] = await Promise.all([
  fetch('/api/usuario/5').then(r => r.json()),
  fetch('/api/perfil/5').then(r => r.json()),
  fetch('/api/preferencias/5').then(r => r.json())
]);

console.log(usuario.nombre, perfil.avatar, prefs.tema);
```

Las tres peticiones se lanzan en paralelo. El tiempo total es el de la más lenta, no la suma.

## Firma

```js
Promise.all(iterable)
```

| Parámetro | Tipo | Descripción |
|---|---|---|
| `iterable` | `Iterable<Promise \| valor>` | Array u otro iterable de Promises. Los valores no-Promise se tratan como `Promise.resolve(valor)`. |

**Retorna:** una Promise que se cumple con un array de valores en el mismo orden del iterable.

## Comportamiento ante casos especiales

| Caso | Resultado |
|---|---|
| Todas se cumplen | `fulfilled` con array de valores (orden del iterable, no de resolución) |
| Alguna se rechaza | `rejected` con la razón de la primera que se rechaza |
| Iterable vacío | `fulfilled` inmediatamente con `[]` |
| Valores no-Promise en el iterable | Se tratan como ya resueltos con ese valor |

## Orden de los resultados

El array de resultados sigue el orden del iterable, no el orden en que las Promises se resuelven. Si la segunda promesa termina antes que la primera, su resultado aún ocupa el índice 1:

```js
const [lento, rapido] = await Promise.all([
  new Promise(res => setTimeout(() => res('lento'), 2000)),
  new Promise(res => setTimeout(() => res('rápido'), 100))
]);

console.log(lento);   // → 'lento'   (índice 0, aunque terminó segundo)
console.log(rapido);  // → 'rápido'  (índice 1, aunque terminó primero)
```

## Fail-fast: comportamiento ante rechazo

En cuanto cualquier Promise se rechaza, la Promise retornada por `Promise.all` se rechaza. Las demás Promises **siguen ejecutándose** (no hay forma de cancelarlas en JS estándar) pero sus resultados se ignoran:

```js
Promise.all([
  Promise.resolve('A'),
  Promise.reject(new Error('B falló')),
  new Promise(res => setTimeout(() => res('C'), 1000)) // sigue corriendo, ignorada
])
.then(valores => console.log(valores))               // no se ejecuta
.catch(err => console.error(err.message));            // → 'B falló'
```

Si se necesita el resultado de todas, incluso las que fallan, usar [[08 Promise.allSettled|Promise.allSettled]].

## Paralelo vs secuencial

La diferencia de rendimiento entre ambos patrones es significativa para múltiples operaciones independientes:

```js
// Secuencial — tiempo total = suma de cada operación
const u = await getUsuario(id);
const p = await getPermisos(u.id);   // espera a getUsuario
const d = await getDatos(p.scope);   // espera a getPermisos
// tiempo ≈ t1 + t2 + t3

// Paralelo con Promise.all — tiempo total = operación más lenta
const [u, p, d] = await Promise.all([getUsuario(id), getPermisos(id), getDatos(id)]);
// tiempo ≈ max(t1, t2, t3)
```

El patrón secuencial se justifica solo cuando cada operación depende del resultado de la anterior.

## Receta: fetch de múltiples recursos en paralelo

```js
async function cargarDashboard(userId) {
  const endpoints = [
    `/api/usuarios/${userId}`,
    `/api/estadisticas/${userId}`,
    `/api/notificaciones/${userId}`
  ];

  const respuestas = await Promise.all(
    endpoints.map(url => fetch(url).then(r => {
      if (!r.ok) throw new Error(`Error ${r.status} en ${url}`);
      return r.json();
    }))
  );

  const [usuario, estadisticas, notificaciones] = respuestas;
  return { usuario, estadisticas, notificaciones };
}
```

## Receta: mapear un array a promesas y esperar todas

```js
const ids = [1, 2, 3, 4, 5];

const usuarios = await Promise.all(
  ids.map(id => fetch(`/api/usuarios/${id}`).then(r => r.json()))
);
// usuarios = [usuario1, usuario2, ..., usuario5] en el orden de ids
```

> [!tip]
> Para limitar la concurrencia (evitar abrir 100 conexiones a la vez), no usar `Promise.all` con todos los elementos. En su lugar, procesar en lotes:
> ```js
> const LOTE = 5;
> for (let i = 0; i < ids.length; i += LOTE) {
>   const lote = ids.slice(i, i + LOTE);
>   await Promise.all(lote.map(id => procesar(id)));
> }
> ```

> [!warning]
> `Promise.all` con fail-fast puede dejar operaciones pendientes sin manejar sus rechazos — si una falla y se ignoran las demás, sus rechazos potenciales dispararán `unhandledrejection`. Para operaciones donde el fallo parcial es esperado, usar `Promise.allSettled` o añadir `.catch` a cada Promise individualmente antes de pasarlas a `Promise.all`.

## Notas relacionadas

- [[index|Promesas — índice de la sección]]
- [[08 Promise.allSettled|Promise.allSettled — sin fail-fast]]
- [[09 Promise.race|Promise.race — la primera en asentarse]]
- [[06 Chaining y Propagación de Errores|Chaining y Propagación de Errores]]
