---
title: Bucles Asíncronos (for await...of) — iterar sobre iterables asíncronos
aliases:
  - for await of
  - for await...of
  - bucles asíncronos
tags:
  - javascript
  - api/concepto
  - asincrono
objeto: global
tipo: concepto
retorna: void
muta: false
asincrono: true
draft: false
order: 5
---

# Bucles Asíncronos (`for await...of`)

> [!definicion]
> `for await (const item of asyncIterable)` itera sobre un **iterable asíncrono** haciendo `await` automático de cada valor antes de ejecutar el cuerpo del bucle. Un iterable asíncrono es cualquier objeto que implementa `[Symbol.asyncIterator]()`, devolviendo un iterador cuyo `.next()` retorna una Promise de `{ value, done }`.

```js
async function procesarStream(url) {
  const res = await fetch(url);
  for await (const chunk of res.body) {
    // chunk es un Uint8Array; el bucle espera cada fragmento del stream
    procesar(chunk);
  }
}
```

## Casos de uso reales

| Fuente | Tipo | Por qué `for await...of` |
|---|---|---|
| `ReadableStream` (Fetch API) | Async iterable nativo | Procesar body por chunks sin acumular en memoria |
| Streams de Node.js (`fs.createReadStream`) | Async iterable nativo | Leer archivos grandes línea a línea |
| Generadores asíncronos (`async function*`) | Async iterable | Paginación de APIs, producción lazy de datos |
| WebSocket / SSE con librería | Async iterable | Consumir mensajes en orden de llegada |

## Diferencia con `for...of` sobre array de Promises

```js
const promesas = [fetch('/a'), fetch('/b'), fetch('/c')];

// for...of no hace await — itera sobre las Promises, no sobre sus valores
for (const p of promesas) {
  console.log(p); // Promise { <pending> }
}

// for await...of sí hace await de cada elemento
for await (const res of promesas) {
  console.log(res.status); // 200, 200, 200
}
// Nota: esto es secuencial — espera cada res antes de iniciar el siguiente await.
// Para paralelismo usar Promise.all + iteración síncrona.
```

## Generadores asíncronos (`async function*`)

Un generador asíncrono combina `yield` (para producir valores) con `await` (para esperar Promises). Produce un iterable asíncrono consumible con `for await...of`.

```js
async function* paginar(url) {
  let cursor = null;
  do {
    const res = await fetch(`${url}?cursor=${cursor ?? ''}`);
    const { datos, nextCursor } = await res.json();
    yield* datos;          // produce cada elemento del array de la página
    cursor = nextCursor;
  } while (cursor);
}

// Consumo — procesa cada ítem a medida que llega, sin cargar todas las páginas
for await (const item of paginar('/api/items')) {
  await procesarItem(item);
}
```

`yield*` delega en un iterable síncrono (el array `datos`), produciendo cada elemento individualmente. El generador pausa en cada `yield` hasta que el consumidor pide el siguiente valor.

## Receta: procesar `ReadableStream` línea a línea

La Fetch API expone `response.body` como `ReadableStream`, que en navegadores modernos es async iterable:

```js
async function* lineasDesdeStream(stream) {
  const reader = stream.getReader();
  const decoder = new TextDecoder();
  let pendiente = '';

  try {
    while (true) {
      const { value, done } = await reader.read();
      if (done) {
        if (pendiente) yield pendiente; // última línea sin \n
        return;
      }
      const texto = decoder.decode(value, { stream: true });
      const fragmentos = (pendiente + texto).split('\n');
      pendiente = fragmentos.pop(); // la última parte puede estar incompleta
      for (const linea of fragmentos) yield linea;
    }
  } finally {
    reader.releaseLock();
  }
}

async function procesarCSV(url) {
  const res = await fetch(url);
  for await (const linea of lineasDesdeStream(res.body)) {
    const campos = linea.split(',');
    registrar(campos);
  }
}
```

## Cómo funciona por dentro

El protocolo de iteración asíncrona es análogo al síncrono pero con Promises:

```js
// El motor transforma:
for await (const x of iterable) { cuerpo(x); }

// Aproximadamente en:
const iterador = iterable[Symbol.asyncIterator]();
let resultado = await iterador.next();
while (!resultado.done) {
  const x = resultado.value;
  cuerpo(x);
  resultado = await iterador.next();
}
```

Cada llamada a `iterador.next()` es awaited, lo que significa que el bucle es inherentemente **secuencial**: termina el cuerpo de una iteración antes de pedir el siguiente valor. Si el orden no importa y la fuente lo permite, `Promise.all` con `map` ofrece paralelismo.

> [!tip]
> Los generadores asíncronos son la herramienta correcta para producir datos de forma lazy: la siguiente página de la API no se solicita hasta que el consumidor ha procesado la actual. Esto evita cargar en memoria todas las páginas o todos los chunks a la vez.

> [!warning]
> `for await...of` solo funciona dentro de funciones `async` o en módulos ES con top-level await. Intentarlo en contexto síncrono produce `SyntaxError`. Además, si el iterable lanza durante la iteración, el error se propaga como una excepción normal y puede capturarse con `try/catch` alrededor del bucle.

## Notas relacionadas

- [[02 await|await]] — el operador subyacente que `for await...of` aplica en cada iteración
- [[01 Función async|Función async]] — contexto requerido para `for await...of`
- [[04 Paralelismo con Promise.all|Paralelismo con Promise.all]] — alternativa cuando el orden no importa y se requiere paralelismo
- [[03 Manejo de Errores (try-catch)|Manejo de Errores con try/catch]] — capturar errores del iterable asíncrono
