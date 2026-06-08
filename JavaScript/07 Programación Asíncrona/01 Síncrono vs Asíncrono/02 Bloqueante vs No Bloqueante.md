---
title: Bloqueante vs No Bloqueante — I/O en JavaScript
aliases:
  - blocking non-blocking
  - bloqueante no bloqueante
  - sync async io
tags:
  - javascript
  - api/concepto
  - asincrono
objeto: global
tipo: concepto
retorna: undefined
muta: false
asincrono: false
draft: false
---

# Bloqueante vs No Bloqueante

> [!definicion]
> **Bloqueante**: el hilo espera a que la operación de I/O termine antes de ejecutar la siguiente instrucción. El Call Stack permanece ocupado durante toda la espera. **No bloqueante**: la operación se delega al entorno; el control retorna al hilo inmediatamente. Cuando la operación termina, el entorno encola el callback/promise para que el Event Loop lo despache.

```js
// --- BLOQUEANTE (Node.js) ---
const fs = require("fs");

const contenido = fs.readFileSync("datos.json", "utf8"); // hilo bloqueado aquí
console.log(contenido);                                   // corre después
console.log("fin");                                       // corre después

// --- NO BLOQUEANTE (Node.js) ---
fs.readFile("datos.json", "utf8", (err, contenido) => {
  console.log(contenido); // corre cuando el SO termina la lectura
});
console.log("fin"); // corre ANTES que el callback
// Output: "fin" → contenido del archivo
```

## Tabla comparativa

| Aspecto | Síncrono / Bloqueante | Asíncrono / No Bloqueante |
|---|---|---|
| Hilo durante la operación | Detenido, esperando | Libre, procesa otros eventos |
| UI durante la operación | Congelada | Responde normalmente |
| Orden de ejecución | Estrictamente secuencial | Determinista pero diferido |
| Manejo de errores | `try/catch` directo | Callbacks `(err, data)` o `.catch()` |
| Ejemplos en Node | `fs.readFileSync`, `execSync` | `fs.readFile`, `fetch`, `setTimeout` |
| Ejemplos en browser | `alert`, `XHR síncrono` (obsoleto) | `fetch`, `addEventListener`, `setTimeout` |
| Complejidad del código | Menor (flujo lineal) | Mayor (callbacks, promesas, async/await) |

## Cuándo es aceptable el modo bloqueante

El modo bloqueante tiene usos legítimos y limitados:

- **Scripts de utilidad CLI** que corren una sola vez y no tienen UI: `fs.readFileSync` en un script de build es razonable.
- **Inicialización al arranque** de un servidor Node antes de empezar a aceptar conexiones: leer un archivo de configuración con `readFileSync` antes del primer `listen()` no bloquea a ningún cliente.
- **Workers** (Web Worker o worker_thread de Node): como tienen su propio hilo, bloquear ese hilo no congela el hilo principal.

Fuera de estos contextos, el modo bloqueante en el hilo principal es un defecto de diseño.

## Cómo funciona por dentro

El modelo no bloqueante funciona porque las operaciones de I/O se delegan al sistema operativo (llamadas `epoll`/`kqueue`/`IOCP` según plataforma), que notifica al proceso JS cuando la operación termina. El entorno (navegador o libuv en Node) actúa como intermediario:

1. JS llama a `fetch(url)` — el motor registra la petición HTTP en el entorno y retorna una Promise pendiente.
2. El hilo JS sigue ejecutando el código siguiente.
3. La respuesta llega al entorno (evento de red del SO).
4. El entorno encola el callback `.then()` en la Microtask Queue.
5. El [[02 Event Loop/index|Event Loop]] despacha el callback cuando el Call Stack está vacío.

El hilo JS nunca estuvo "esperando" — estaba libre para manejar otros eventos.

## XHR síncrono — legado a evitar

`XMLHttpRequest` admite un tercer argumento `false` para modo síncrono:

```js
// ❌ Obsoleto y prohibido en workers — congela el hilo
const xhr = new XMLHttpRequest();
xhr.open("GET", "/api/data", false); // false = síncrono
xhr.send();
console.log(xhr.responseText); // disponible inmediatamente, pero bloqueó la UI
```

Los navegadores modernos lanzan un warning en la consola y el estándar WHATWG lo marca como deprecated. En Service Workers está prohibido y lanza excepción.

> [!tip]
> La forma de distinguir si una API es bloqueante o no: si retorna el valor directamente (`const data = api()`), bloquea. Si retorna una Promise o acepta un callback (`api(url).then(...)` o `api(url, callback)`), no bloquea.

> [!warning]
> El modo no bloqueante no elimina los errores — los desplaza. Un error en una operación asíncrona que no tiene `.catch()` o manejo de error en el callback se convierte en una Promise rechazada sin manejar (`UnhandledPromiseRejection`), que en Node.js termina el proceso.

## Notas relacionadas

- [[01 Single-Threaded|Single-Threaded]]
- [[02 Event Loop/index|Event Loop]]
- [[02 Event Loop/02 Web APIs|Web APIs]]
- [[02 Event Loop/04 Microtask Queue|Microtask Queue]]
