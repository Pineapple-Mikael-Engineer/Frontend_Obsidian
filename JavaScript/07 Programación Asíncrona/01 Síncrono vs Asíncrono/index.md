---
title: Síncrono vs Asíncrono — Modelo de ejecución JavaScript
aliases:
  - sincrono asincrono
  - sync async js
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

# Síncrono vs Asíncrono

> [!definicion]
> **Síncrono**: el hilo de ejecución espera a que cada operación termine antes de avanzar a la siguiente. **Asíncrono**: la operación se delega al entorno; el hilo continúa y recibe la notificación cuando la operación termina.

La distinción no es de JavaScript como lenguaje, sino de cómo se relaciona el motor JS con el entorno (navegador / Node.js): las operaciones lentas (I/O de red, disco, timers) se ejecutan fuera del hilo JS y notifican mediante callbacks, promesas o eventos.

```js
// Síncrono — el hilo espera
const data = fs.readFileSync("file.txt", "utf8"); // bloquea
console.log(data);                                 // corre después

// Asíncrono — el hilo continúa
fs.readFile("file.txt", "utf8", (err, data) => {
  console.log(data); // corre cuando el SO termina
});
console.log("esto corre ANTES que el callback");
```

## Por qué JavaScript necesita asincronía

JavaScript tiene un **único hilo de ejecución** — no puede correr dos fragmentos de código al mismo tiempo. Esto es una ventaja (sin race conditions en el estado compartido), pero implica que cualquier operación bloqueante detiene toda la aplicación: la UI no responde, los eventos se acumulan sin procesar, las animaciones se congelan.

Las operaciones de I/O son órdenes de magnitud más lentas que la CPU:

| Operación | Latencia típica |
|---|---|
| Acceso a registro de CPU | ~0.3 ns |
| Acceso a RAM | ~100 ns |
| SSD local | ~100 µs |
| Red (mismo datacenter) | ~0.5 ms |
| Red (intercontinental) | ~100–300 ms |

Hacer esas operaciones de forma bloqueante desaprovecha la CPU y congela la experiencia. El modelo asíncrono permite que el hilo JS siga procesando eventos mientras el entorno espera la respuesta de la red o el disco.

## Cómo se organiza la asincronía

El mecanismo que orquesta las notificaciones es el [[02 Event Loop/index|Event Loop]]. Sus componentes:

- [[02 Event Loop/01 Call Stack|Call Stack]] — pila de ejecución síncrona.
- [[02 Event Loop/02 Web APIs|Web APIs]] — operaciones delegadas al entorno.
- [[02 Event Loop/03 Task Queue (Macrotasks)|Task Queue]] — callbacks de `setTimeout`, I/O, eventos.
- [[02 Event Loop/04 Microtask Queue|Microtask Queue]] — callbacks de Promises y `queueMicrotask`.

## Notas relacionadas

- [[01 Single-Threaded|Single-Threaded — un solo hilo de ejecución]]
- [[02 Bloqueante vs No Bloqueante|Bloqueante vs No Bloqueante]]
- [[02 Event Loop/index|Event Loop]]
