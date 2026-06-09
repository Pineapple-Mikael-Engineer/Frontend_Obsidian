---
title: IndexedDB — base de datos NoSQL en el navegador
aliases:
  - IndexedDB
  - IDB
tags:
  - javascript
  - api/web
  - almacenamiento
draft: false
---

# IndexedDB

> [!definicion]
> **IndexedDB** es una base de datos NoSQL orientada a objetos integrada en el navegador. Almacena pares clave-valor donde los valores son objetos JavaScript completos (structured clone); soporta blobs, archivos y datos binarios sin serialización a string. La capacidad es sustancialmente mayor que Web Storage (decenas a cientos de MB según el navegador y el espacio disponible). La API nativa es asíncrona basada en eventos; la librería `idb` de Jake Archibald la envuelve en Promises modernas.

```js
import { openDB } from "idb";

const db = await openDB("miApp", 1, {
  upgrade(db) {
    const store = db.createObjectStore("usuarios", { keyPath: "id" });
    store.createIndex("email", "email", { unique: true });
  }
});

await db.put("usuarios", { id: 1, nombre: "Ana", email: "ana@example.com" });
const usuario = await db.get("usuarios", 1); // { id: 1, nombre: "Ana", ... }
```

IndexedDB organiza los datos en **object stores** (equivalentes a tablas) agrupados en una **base de datos** con nombre y número de versión. Toda lectura y escritura ocurre dentro de una **transacción**, que garantiza atomicidad y consistencia (ACID). Los cambios estructurales (crear/borrar stores e índices) solo ocurren en el evento `upgradeneeded`.

## Bloques de esta sección

- [[01 Conceptos (stores, índices)|Conceptos: stores, índices y versiones]] — apertura de la BD, object stores, keyPath, autoIncrement, índices y el ciclo de vida de la versión.
- [[02 Transacciones|Transacciones y operaciones CRUD]] — modos de transacción, métodos del store (`add`, `put`, `get`, `delete`), cursores, búsqueda por índice y la librería `idb`.

## Comparativa con Web Storage

| Característica | localStorage | IndexedDB |
|---|---|---|
| Capacidad | ~5-10 MB | Decenas-cientos de MB |
| Tipos de valor | Solo strings | Objetos JS, blobs, archivos |
| API | Síncrona | Asíncrona (eventos / Promises) |
| Índices / búsquedas | No | Sí, por cualquier propiedad |
| Transacciones ACID | No | Sí |
| Disponible en SW | No | Sí |

> [!tip]
> Para proyectos nuevos, usar la librería [`idb`](https://github.com/jakearchibald/idb) en lugar de la API nativa. Elimina el boilerplate de `onsuccess`/`onerror` y convierte todas las operaciones en Promises que se integran con `async/await`.

> [!warning]
> IndexedDB tiene una curva de aprendizaje pronunciada con la API nativa: `indexedDB.open()` devuelve un `IDBOpenDBRequest`, no una Promise. Mezclar el modelo de eventos con async/await sin una librería suele producir bugs difíciles de rastrear.

## Notas relacionadas

- [[../02 Web Storage/index|Web Storage]] — alternativa síncrona para datos pequeños en formato string
- [[../04 Cache API|Cache API]] — almacenamiento de pares Request/Response para assets estáticos (PWA)
- [[../index|09 Almacenamiento en Cliente]] — visión comparativa de todos los mecanismos de almacenamiento
