---
title: Manejo de Errores con try/catch — capturar rechazos en async/await
aliases:
  - try catch async
  - manejo de errores async await
tags:
  - javascript
  - api/concepto
  - asincrono
objeto: global
tipo: concepto
retorna: any
muta: false
asincrono: true
draft: false
order: 3
---

# Manejo de Errores (`try/catch`) en `async/await`

> [!definicion]
> Cuando una Promise awaited se rechaza, el `await` lanza la razón del rechazo como una excepción síncrona dentro de la función `async`. Un bloque `try/catch` convencional la captura exactamente igual que cualquier error síncrono. `finally` se ejecuta siempre, independientemente de si hubo error o no.

```js
async function cargarUsuario(id) {
  try {
    const res = await fetch(`/api/users/${id}`);
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    return await res.json();
  } catch (err) {
    console.error('Error al cargar usuario:', err.message);
    throw err; // re-lanzar para que el caller lo maneje
  } finally {
    console.log('fetch completado (con o sin error)');
  }
}
```

## Equivalencia con `.catch()`

`try/catch` en `async/await` y `.catch()` en Promises son semánticamente equivalentes. La elección es de estilo:

| Con async/await | Con .then()/.catch() | Diferencia |
|---|---|---|
| `try { await p } catch(e) {}` | `p.catch(e => {})` | Ninguna — misma semántica |
| `try { const x = await p1; const y = await p2 } catch(e) {}` | `p1.then(x => p2).catch(e => {})` | Un solo catch para ambas |
| `finally {}` | `.finally(() => {})` | Ninguna |

## Granularidad del `try/catch`

La granularidad del bloque `try` controla qué errores se capturan juntos. Hay un trade-off entre brevedad y precisión:

```js
// 🔵 try/catch amplio — un solo punto de captura, menos verboso
async function procesarPedido(id) {
  try {
    const pedido = await obtenerPedido(id);
    const usuario = await obtenerUsuario(pedido.userId);
    await enviarNotificacion(usuario.email);
    return { ok: true };
  } catch (err) {
    // No se sabe qué operación falló sin inspeccionar err
    logError(err);
    return { ok: false, error: err.message };
  }
}

// 🔵 try/catch granular — control fino por operación
async function procesarPedidoV2(id) {
  let pedido;
  try {
    pedido = await obtenerPedido(id);
  } catch (err) {
    throw new Error(`Pedido ${id} no encontrado: ${err.message}`);
  }

  const usuario = await obtenerUsuario(pedido.userId); // deja propagar
  await enviarNotificacion(usuario.email);
  return { ok: true };
}
```

> [!tip]
> La regla práctica: usar `try/catch` amplio cuando todos los errores del bloque tienen el mismo tratamiento (loguear + fallback); usar bloques granulares cuando distintas operaciones requieren manejo diferenciado o mensajes de error específicos.

## `finally` — ejecución garantizada

```js
async function descargarArchivo(url) {
  let handle;
  try {
    handle = await abrirConexion(url);
    const datos = await handle.leer();
    return datos;
  } catch (err) {
    console.error('Descarga fallida:', err);
    throw err;
  } finally {
    if (handle) await handle.cerrar(); // se ejecuta siempre
  }
}
```

`finally` es el lugar correcto para limpiar recursos (cerrar conexiones, ocultar spinners, liberar locks) independientemente del resultado.

## Patrón alternativo: captura inline con `.catch()`

Para un solo `await`, capturar el rechazo directamente en la expresión evita un bloque `try/catch` y permite un valor por defecto limpio:

```js
async function obtenerConfig() {
  const config = await cargarRemota().catch(() => CONFIG_DEFAULT);
  return config; // nunca rechaza — siempre hay un valor
}
```

## Patrón Go-style: función utilitaria `to(promise)`

Inspirado en el manejo de errores de Go, evita el `try/catch` anidado cuando hay múltiples operaciones independientes que pueden fallar:

```js
// Utilidad genérica
function to(promise) {
  return promise.then(data => [null, data]).catch(err => [err, null]);
}

// Uso
async function sincronizar(id) {
  const [errUsuario, usuario] = await to(obtenerUsuario(id));
  if (errUsuario) return manejarErrorUsuario(errUsuario);

  const [errPedidos, pedidos] = await to(obtenerPedidos(id));
  if (errPedidos) return manejarErrorPedidos(errPedidos);

  return { usuario, pedidos };
}
```

## Patrón: retry automático

```js
async function conReintento(fn, intentos = 3, delay = 500) {
  for (let i = 0; i < intentos; i++) {
    try {
      return await fn();
    } catch (err) {
      if (i === intentos - 1) throw err; // agotados los intentos
      await new Promise(res => setTimeout(res, delay * (i + 1))); // backoff lineal
    }
  }
}

// Uso
const datos = await conReintento(() => fetch('/api/datos').then(r => r.json()));
```

> [!warning]
> Un `throw` dentro de `finally` anula cualquier error que venía del `catch` — el error original se descarta silenciosamente. Dentro de `finally`, solo lanzar si hay un error de limpieza real; en caso contrario, loguear y continuar.

## Errores no capturados en funciones async

Una función `async` que rechaza sin ser `await`-ada produce un rechazo de Promise no manejado. En navegadores modernos y Node.js esto emite un warning y puede terminar el proceso:

```js
// ❌ El error de procesarDatos() se pierde — nadie hace await
function iniciar() {
  procesarDatos(); // Promise rechazada sin captura
}

// ✅ Capturar siempre — con await o con .catch()
function iniciar() {
  procesarDatos().catch(err => logError(err));
}
```

## Notas relacionadas

- [[02 await|await]] — el operador que puede lanzar el error
- [[01 Función async|Función async]] — contexto donde opera try/catch
- [[04 Paralelismo con Promise.all|Paralelismo con Promise.all]] — manejo de fallos parciales con `allSettled`
- [[05 Promesas/index|Promesas]] — `.catch()` y `.finally()` equivalentes
