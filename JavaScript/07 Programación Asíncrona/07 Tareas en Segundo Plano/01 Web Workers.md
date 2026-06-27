---
title: Web Workers — hilo de ejecución separado para cómputo intensivo
aliases:
  - Web Worker
  - Worker dedicado
  - web worker
tags:
  - javascript
  - api/web
  - asincrono
objeto: Web API
tipo: concepto
retorna: Worker
muta: false
asincrono: true
draft: false
order: 1
---

# Web Workers

> [!definicion]
> Un **Web Worker** es un script que el navegador ejecuta en un hilo del sistema operativo separado del hilo principal de JavaScript. Carece de acceso al DOM, a `window` y a `document`, pero puede realizar cómputo arbitrario (cifrado, análisis de datos, procesamiento de imágenes, simulaciones) sin bloquear la interfaz de usuario. La comunicación con el hilo principal se realiza exclusivamente mediante paso de mensajes.

```js
// hilo principal
const worker = new Worker("worker.js");

worker.postMessage({ tipo: "calcular", datos: granArray });

worker.onmessage = (e) => {
  console.log("Resultado:", e.data); // → valor calculado por el worker
};

worker.onerror = (err) => {
  console.error(err.message);
};
```

```js
// worker.js — corre en hilo separado
self.onmessage = (e) => {
  const { tipo, datos } = e.data;
  if (tipo === "calcular") {
    const resultado = datos.reduce((acc, n) => acc + n, 0); // cómputo intensivo aquí
    self.postMessage(resultado);
  }
};
```

## Crear un Worker

| Forma | Código | Cuándo usarla |
|---|---|---|
| Archivo externo | `new Worker("worker.js")` | Producción, worker reutilizable |
| Blob inline | `new Worker(URL.createObjectURL(blob))` | Prototipado, worker generado dinámicamente |
| Módulo ES | `new Worker("worker.js", { type: "module" })` | Necesitar `import` dentro del worker |

```js
// Worker inline con Blob
const codigo = `
  self.onmessage = (e) => self.postMessage(e.data * 2);
`;
const blob = new Blob([codigo], { type: "application/javascript" });
const worker = new Worker(URL.createObjectURL(blob));
```

## Comunicación: postMessage y onmessage

El canal de comunicación es **asimétrico**: el hilo principal usa la instancia `Worker`; el worker usa el objeto global `self`.

| Dirección | Enviar | Recibir |
|---|---|---|
| Principal → Worker | `worker.postMessage(datos)` | `self.onmessage = e => e.data` |
| Worker → Principal | `self.postMessage(resultado)` | `worker.onmessage = e => e.data` |

Los datos se transfieren por **structured clone** (copia profunda automática): números, strings, arrays, objetos planos, `ArrayBuffer`, `Blob`, `ImageData`, `Map`, `Set`. Las funciones y prototipos personalizados no se pueden clonar.

## Transferencia vs Copia (Transferable Objects)

Para grandes buffers, la copia tiene coste lineal. La **transferencia** mueve la propiedad del buffer al receptor en O(1) — el emisor pierde acceso.

```js
// hilo principal — transfiere el ArrayBuffer (no lo copia)
const buffer = new ArrayBuffer(1024 * 1024 * 64); // 64 MB
worker.postMessage({ buffer }, [buffer]); // segundo argumento: lista de transferibles
// buffer ya no es accesible en el hilo principal
```

Objetos transferibles: `ArrayBuffer`, `MessagePort`, `OffscreenCanvas`, `ImageBitmap`, `ReadableStream`, `WritableStream`.

## Ciclo de vida: terminate y close

```js
// Desde el hilo principal — detención inmediata, sin cleanup
worker.terminate();

// Desde el worker — el worker se detiene a sí mismo de forma limpia
self.close();
```

`terminate()` es abrupto: cualquier mensaje pendiente en la cola se descarta.

## APIs disponibles en el Worker

El worker no tiene DOM pero tiene acceso a una parte significativa de las Web APIs:

| Disponible | No disponible |
|---|---|
| `fetch`, `WebSocket` | `window`, `document` |
| `IndexedDB` | `localStorage`, `sessionStorage` |
| `setTimeout`, `setInterval` | `alert`, `confirm` |
| `crypto.subtle` | `navigator.geolocation` |
| `console`, `performance` | Cualquier API que requiera UI |
| `importScripts()` (clásico) | Manipulación de CSS/DOM |

## Memoria compartida: SharedArrayBuffer + Atomics

La transferencia es unidireccional (un hilo pierde el buffer). Para comunicación bidireccional en tiempo real se usa `SharedArrayBuffer`: ambos hilos leen y escriben el mismo bloque de memoria.

```js
// hilo principal
const sab = new SharedArrayBuffer(4); // 4 bytes compartidos
const vista = new Int32Array(sab);
worker.postMessage({ sab }); // se comparte, no se copia ni transfiere

// escritura atómica para evitar condiciones de carrera
Atomics.store(vista, 0, 42);
Atomics.notify(vista, 0, 1); // despierta al worker que espera en índice 0
```

```js
// worker.js
self.onmessage = (e) => {
  const vista = new Int32Array(e.data.sab);
  Atomics.wait(vista, 0, 0);        // espera hasta que índice 0 ≠ 0
  console.log(Atomics.load(vista, 0)); // → 42
};
```

`SharedArrayBuffer` requiere que el documento se sirva con las cabeceras `Cross-Origin-Opener-Policy: same-origin` y `Cross-Origin-Embedder-Policy: require-corp`.

## Tipos de Workers

| Tipo | Constructor | Alcance | Caso de uso |
|---|---|---|---|
| Dedicado | `new Worker()` | Una sola página | Cómputo aislado por pestaña |
| Compartido | `new SharedWorker()` | Múltiples pestañas del mismo origen | Estado compartido entre pestañas |
| Service Worker | `navigator.serviceWorker.register()` | Origen completo, persiste | Proxy de red, caché offline, push |

## Cómo funciona por dentro

El navegador lanza un thread del SO para el Worker. Este thread tiene su propio V8 isolate (heap separado), por lo que el recolector de basura de cada hilo es independiente. El canal `postMessage` implementa la especificación de **structured clone algorithm** y opera sobre una cola de mensajes; los mensajes se procesan en orden FIFO en el event loop del worker, que es idéntico en estructura al del hilo principal (call stack + task queue + microtask queue) pero sin integración con el rendering pipeline.

## Receta — Hash SHA-256 de un archivo grande

```js
// worker.js
self.onmessage = async (e) => {
  const { buffer } = e.data;
  const hashBuffer = await crypto.subtle.digest("SHA-256", buffer);
  const hex = Array.from(new Uint8Array(hashBuffer))
    .map(b => b.toString(16).padStart(2, "0"))
    .join("");
  self.postMessage({ hex });
};
```

```js
// hilo principal
async function hashearArchivo(file) {
  const worker = new Worker("worker.js");
  const buffer = await file.arrayBuffer();
  return new Promise((resolve, reject) => {
    worker.onmessage = (e) => { resolve(e.data.hex); worker.terminate(); };
    worker.onerror = reject;
    worker.postMessage({ buffer }, [buffer]); // transferencia O(1)
  });
}
```

## Receta — Pattern matching en dataset grande sin congelar la UI

```js
// worker.js — búsqueda sin bloquear la UI
self.onmessage = (e) => {
  const { dataset, patron } = e.data;
  const regex = new RegExp(patron, "gi");
  const resultados = dataset.filter(item => regex.test(item.texto));
  self.postMessage({ resultados });
};
```

```js
// hilo principal
const worker = new Worker("worker.js");
inputBusqueda.addEventListener("input", () => {
  worker.postMessage({ dataset: datos, patron: inputBusqueda.value });
});
worker.onmessage = (e) => renderizarResultados(e.data.resultados);
```

> [!tip]
> Para cómputo que tarda más de ~50 ms, un Worker es casi siempre la elección correcta. El umbral perceptual del usuario para bloqueo de UI es 100 ms; todo lo que supere ese tiempo sin Worker producirá jank visible.

> [!warning]
> Los Workers tienen coste de arranque (20–100 ms según plataforma). Para tareas muy cortas y frecuentes, mantener un pool de Workers pre-lanzados (o usar `comlink` de Google) es más eficiente que crear y terminar workers en cada llamada. Tampoco compartir referencias a objetos mutables vía `postMessage`; el structured clone los copia, no los referencia.

## Notas relacionadas

- [[02 Service Workers|Service Workers]] — worker especial con ciclo de vida propio y acceso a la Cache API
- [[07 Tareas en Segundo Plano/index|Tareas en Segundo Plano]] — panorama de Workers en JS
- [[02 Event Loop/index|Event Loop]] — cómo el hilo principal procesa mensajes del worker
