---
title: Paralelismo con Promise.all — ejecutar Promises en paralelo con await
aliases:
  - Promise.all async
  - paralelismo async await
tags:
  - javascript
  - api/concepto
  - asincrono
objeto: Promise
tipo: concepto
retorna: Promise
muta: false
asincrono: true
draft: false
---

# Paralelismo con `Promise.all`

> [!definicion]
> El error más frecuente con `async/await` es ejecutar Promises de forma secuencial cuando podrían correr en paralelo. `await` en sentencias independientes hace que cada operación espere a que termine la anterior, multiplicando los tiempos de espera. `Promise.all` arranca todas las Promises simultáneamente y espera a que todas se resuelvan, tardando solo el máximo de los tiempos individuales.

```js
// ❌ Secuencial — tarda suma de tiempos (ej. 300ms + 200ms = 500ms)
const usuario  = await getUsuario(id);
const pedidos  = await getPedidos(id);
const stats    = await getStats(id);

// ✅ Paralelo — tarda el máximo (ej. max(300ms, 200ms, 150ms) = 300ms)
const [usuario, pedidos, stats] = await Promise.all([
  getUsuario(id),
  getPedidos(id),
  getStats(id),
]);
```

## Cuándo secuencial es correcto

La secuencialidad es necesaria cuando la segunda operación depende del resultado de la primera:

```js
// ✅ Correcto ser secuencial — el pedido necesita el userId del usuario
const usuario = await getUsuario(id);
const pedidos = await getPedidosPorUser(usuario.userId); // depende de usuario
```

Si las operaciones son independientes, el paralelismo siempre es preferible.

## `Promise.allSettled` — fallos parciales

`Promise.all` rechaza en cuanto cualquier Promise falla, descartando los demás resultados. `Promise.allSettled` espera a que todas se asienten y reporta el resultado individual de cada una:

```js
const resultados = await Promise.allSettled([
  getUsuario(id),
  getPedidos(id),
  getNotificaciones(id),
]);

resultados.forEach(r => {
  if (r.status === 'fulfilled') {
    console.log('OK:', r.value);
  } else {
    console.warn('Fallo:', r.reason.message);
  }
});
```

| Método | Falla si | Retorna |
|---|---|---|
| `Promise.all` | Cualquier Promise rechaza | Array de valores o el primer error |
| `Promise.allSettled` | Nunca rechaza | Array de `{status, value/reason}` |
| `Promise.any` | Todas rechazan | El primer valor fulfilled o `AggregateError` |
| `Promise.race` | La primera en asentarse rechaza | Valor o error del primero en asentarse |

## `Promise.any` — fallback a múltiples fuentes

```js
// Intentar obtener config desde múltiples fuentes; basta con una exitosa
const config = await Promise.any([
  fetch('/config-primary.json').then(r => r.json()),
  fetch('/config-backup.json').then(r => r.json()),
  Promise.resolve(CONFIG_DEFAULT),
]);
```

## Receta: cargar datos de dashboard en paralelo

```js
async function cargarDashboard(userId) {
  const [usuario, stats, notificaciones] = await Promise.all([
    api.getUsuario(userId),
    api.getStats(userId),
    api.getNotificaciones(userId),
  ]);

  return { usuario, stats, notificaciones };
}
```

Tres peticiones de red en paralelo; el tiempo total es el de la más lenta, no la suma de las tres.

## Receta: procesar array en paralelo con límite de concurrencia

Lanzar `Promise.all` sobre un array grande puede saturar recursos (conexiones, rate limits). Un semáforo limita el número de Promises activas simultáneamente:

```js
async function mapConConcurrencia(items, fn, limite = 5) {
  const resultados = [];
  const cola = [...items];
  const workers = Array.from({ length: Math.min(limite, items.length) }, async () => {
    while (cola.length > 0) {
      const item = cola.shift();
      resultados.push(await fn(item));
    }
  });
  await Promise.all(workers);
  return resultados;
}

// Uso: procesar 100 URLs con máximo 5 peticiones activas a la vez
const respuestas = await mapConConcurrencia(urls, url =>
  fetch(url).then(r => r.json()), 5);
```

> [!warning]
> `Promise.all` lanza en cuanto una Promise rechaza, pero las Promises restantes **siguen ejecutándose** — no se cancelan. Si es necesario cancelar operaciones en curso, usar `AbortController` para las peticiones de red o un flag de cancelación para operaciones internas.

> [!tip]
> El patrón `await Promise.all([...])` con destructuring es idiomático y claro: nombra cada valor en su posición y deja explícito que todas las operaciones son independientes. Es preferible a acumular en variables separadas con múltiples `await` sin `Promise.all`.

## Notas relacionadas

- [[02 await|await]] — el anti-patrón secuencial que este módulo corrige
- [[03 Manejo de Errores (try-catch)|Manejo de Errores con try/catch]] — capturar fallos de `Promise.all`
- [[05 Promesas/index|Promesas]] — `Promise.all`, `Promise.allSettled`, `Promise.any`, `Promise.race`
- [[05 Bucles Asíncronos (for await...of)|for await...of]] — alternativa para arrays grandes con control de flujo
