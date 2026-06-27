---
title: await — pausar ejecución hasta que una Promise se asiente
aliases:
  - await
  - operador await
tags:
  - javascript
  - api/operador
  - asincrono
objeto: global
tipo: operador
retorna: any
muta: false
asincrono: true
draft: false
order: 2
---

# `await`

> [!definicion]
> `await expresión` pausa la ejecución de la función `async` que la contiene hasta que la Promise producida por `expresión` se asienta. Si la Promise se cumple, devuelve el valor fulfillado; si se rechaza, lanza el error (que puede capturarse con `try/catch`). Solo es válido dentro de funciones `async` o en módulos ES con top-level await (ES2022).

```js
async function ejemplo() {
  const respuesta = await fetch('/api/datos');   // pausa hasta Response
  const datos = await respuesta.json();          // pausa hasta Object
  return datos;                                  // retorna Promise<Object>
}
```

## Comportamiento según el tipo de expresión

| Expresión después de `await` | Comportamiento | Cuándo se reanuda |
|---|---|---|
| `Promise` pendiente | Suspende la función, cede al Event Loop | Cuando la Promise se asienta |
| `Promise` ya resuelta | Encola microtask de reanudación | En la siguiente microtask |
| Valor primitivo (`42`, `"hola"`) | Envuelto en `Promise.resolve(42)` | En la siguiente microtask |
| `null` / `undefined` | Envuelto en `Promise.resolve(undefined)` | En la siguiente microtask |
| Thenable (objeto con `.then`) | Se adopta como Promise | Cuando su `.then` llama al callback |

```js
async function tiposDeAwait() {
  const a = await 42;                        // → 42  (microtask, no bloquea long)
  const b = await Promise.resolve('hola');   // → 'hola'
  const c = await new Promise(res => setTimeout(res, 100, 'tardé')); // → 'tardé'
  console.log(a, b, c); // 42 hola tardé
}
```

> [!info]
> Incluso `await valorSíncrono` no se resuelve en el mismo tick: el motor siempre encola al menos una microtask. Esto garantiza que el comportamiento sea predecible y que el código después del `await` nunca se ejecute síncronamente en la misma llamada.

## Cada `await` cede el Event Loop

```js
async function ordenDeEjecucion() {
  console.log('1 — antes del await');
  const val = await Promise.resolve('X');
  console.log('3 — reanudado, val =', val);
}

ordenDeEjecucion();
console.log('2 — código síncrono posterior');

// Salida:
// 1 — antes del await
// 2 — código síncrono posterior
// 3 — reanudado, val = X
```

La función `async` retorna en el punto del `await` y el script síncrono continúa. La reanudación ocurre después de que el call stack se vacía, en la fase de microtasks.

## Múltiples Promises en paralelo

```js
// ❌ Secuencial — espera una, luego la otra (suma de tiempos)
const usuario = await getUsuario(id);
const pedidos = await getPedidos(id);

// ✅ Paralelo — ambas arrancadas antes del primer await
const [usuario, pedidos] = await Promise.all([getUsuario(id), getPedidos(id)]);
```

El detalle completo de paralelismo se desarrolla en [[04 Paralelismo con Promise.all|Paralelismo con Promise.all]].

## Anti-patrón: `await` en `forEach`

```js
// ❌ forEach NO espera — los callbacks son funciones async independientes,
//    forEach no hace await de su retorno
const ids = [1, 2, 3];
ids.forEach(async (id) => {
  const user = await getUsuario(id);
  console.log(user.name); // orden indefinido, forEach ya terminó
});
console.log('forEach terminado'); // se imprime antes que los nombres

// ✅ Opción 1: for...of — espera cada iteración en orden
for (const id of ids) {
  const user = await getUsuario(id);
  console.log(user.name);
}

// ✅ Opción 2: Promise.all + map — paralelo, sin perder el await
const users = await Promise.all(ids.map(async id => getUsuario(id)));
users.forEach(u => console.log(u.name));
```

> [!warning]
> `Array.prototype.forEach`, `Array.prototype.reduce` y similares no están diseñados para funciones `async`. El callback retorna una Promise que `forEach` ignora silenciosamente, por lo que los errores también se pierden (rechazo no capturado). Usar `for...of` con `await` o `Promise.all(array.map(async …))`.

## Cómo funciona por dentro

Cada `await` es equivalente a registrar el resto de la función como callback de `.then()`. El motor divide la función en fragmentos en cada punto de suspensión:

```js
// Con await:
async function procesar() {
  const a = await paso1();
  const b = await paso2(a);
  return b;
}

// Equivalencia aproximada con Promises:
function procesar() {
  return paso1().then(a => paso2(a)).then(b => b);
}
```

La diferencia real es que `async/await` preserva el scope local completo (variables `a`, `b`) entre suspensiones, mientras que con `.then()` es necesario anidar o capturar en closure manualmente.

> [!tip]
> `await` puede usarse en cualquier expresión donde se esperaría un valor: `const doble = (await obtenerNumero()) * 2`, aunque por claridad suele separarse en dos líneas. En condiciones: `if (await esValido(x)) { … }`.

## Notas relacionadas

- [[01 Función async|Función async]] — contexto donde `await` es válido
- [[03 Manejo de Errores (try-catch)|Manejo de Errores con try/catch]] — capturar rechazos de `await`
- [[04 Paralelismo con Promise.all|Paralelismo con Promise.all]] — evitar secuencialidad innecesaria
- [[05 Bucles Asíncronos (for await...of)|for await...of]] — iteración asíncrona
- [[04 Microtask Queue|Microtask Queue]] — modelo de ejecución de la reanudación
