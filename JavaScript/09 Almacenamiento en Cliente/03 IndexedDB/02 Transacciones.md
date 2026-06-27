---
title: IndexedDB — Transacciones y operaciones CRUD
aliases:
  - IDB transacciones
  - IndexedDB CRUD
  - IDBTransaction
tags:
  - javascript
  - api/web
  - almacenamiento
objeto: IDBDatabase
tipo: concepto
asincrono: true
draft: false
order: 2
---

# IndexedDB — Transacciones y operaciones CRUD

> [!definicion]
> Toda operación de lectura o escritura en IndexedDB ocurre dentro de una **transacción** (`IDBTransaction`). Una transacción se crea con `db.transaction(storeNames, mode)`, donde `mode` es `"readonly"` o `"readwrite"`. Las transacciones son **ACID**: atómicas (todas las operaciones se confirman o ninguna), consistentes, aisladas (no hay lecturas sucias entre transacciones concurrentes) y durables (los datos persisten en disco). La transacción se confirma automáticamente cuando no quedan operaciones pendientes en ella — no existe un `commit()` explícito en la API nativa.

```js
// Receta básica con la API nativa
const tx    = db.transaction("usuarios", "readwrite");
const store = tx.objectStore("usuarios");

store.put({ id: 1, nombre: "Ana", email: "ana@example.com" });
store.put({ id: 2, nombre: "Luis", email: "luis@example.com" });

tx.oncomplete = () => console.log("guardado");
tx.onerror    = e  => console.error(e.target.error);
```

## Operaciones del object store

| Método | Descripción | Comportamiento si la clave existe |
|---|---|---|
| `store.add(obj)` | Insertar nuevo registro | Lanza error (clave duplicada) |
| `store.put(obj)` | Insertar o actualizar (upsert) | Reemplaza el registro existente |
| `store.get(key)` | Obtener por clave primaria | `undefined` si no existe |
| `store.getAll(query?, count?)` | Obtener todos (o filtrados por rango) | Array vacío si no hay coincidencias |
| `store.delete(key)` | Eliminar por clave primaria | Silencioso si no existe |
| `store.clear()` | Eliminar todos los registros | — |
| `store.count(query?)` | Contar registros | — |
| `store.openCursor(query?, dir?)` | Iterar con cursor | — |

Cada método devuelve un `IDBRequest`; su resultado queda en `request.onsuccess` → `e.target.result`.

## Con la librería `idb`

La librería `idb` de Jake Archibald envuelve la API en Promises y elimina el boilerplate de eventos. Es la forma recomendada para código moderno.

```js
import { openDB } from "idb";

const db = await openDB("miApp", 1, {
  upgrade(db) {
    const store = db.createObjectStore("usuarios", { keyPath: "id" });
    store.createIndex("por-email", "email", { unique: true });
  }
});

// Escritura — put (upsert)
await db.put("usuarios", { id: 1, nombre: "Ana", email: "ana@example.com" });

// Lectura por clave primaria
const usuario = await db.get("usuarios", 1);
// → { id: 1, nombre: "Ana", email: "ana@example.com" }

// Leer todos
const todos = await db.getAll("usuarios");

// Eliminar
await db.delete("usuarios", 1);

// Transacción explícita para múltiples operaciones
const tx = db.transaction("usuarios", "readwrite");
await Promise.all([
  tx.store.put({ id: 3, nombre: "Marta", email: "marta@example.com" }),
  tx.store.put({ id: 4, nombre: "Rafa",  email: "rafa@example.com"  }),
  tx.done
]);
```

## Búsqueda por índice

Para buscar por una propiedad distinta a la clave primaria se accede al índice dentro de la transacción:

```js
// API nativa
const tx    = db.transaction("usuarios", "readonly");
const index = tx.objectStore("usuarios").index("por-email");
const req   = index.get("ana@example.com");
req.onsuccess = e => console.log(e.target.result);

// Con idb
const usuario = await db.getFromIndex("usuarios", "por-email", "ana@example.com");
const varios  = await db.getAllFromIndex("usuarios", "por-ciudad", "Madrid");
```

## Cursores

`store.openCursor()` itera sobre todos los registros en orden de clave primaria. `cursor.continue()` avanza al siguiente registro.

```js
// Con idb: usando un cursor para procesar registro a registro
const tx     = db.transaction("usuarios", "readonly");
let   cursor = await tx.store.openCursor();

while (cursor) {
  console.log(cursor.key, cursor.value);
  cursor = await cursor.continue();
}
```

Los cursores son útiles cuando se necesita transformar o filtrar registros sin cargarlos todos en memoria con `getAll`.

## Rangos de clave (IDBKeyRange)

Las operaciones de lectura y los cursores aceptan un `IDBKeyRange` como argumento `query` para limitar el conjunto de registros:

```js
// Registros con id entre 10 y 20 (inclusive)
const rango = IDBKeyRange.bound(10, 20);
const subset = await db.getAll("usuarios", rango);

// Registros con id > 5
const mayores = await db.getAll("usuarios", IDBKeyRange.lowerBound(5, true));
```

## Eventos de transacción

Con la API nativa, una transacción expone tres eventos:

- **`oncomplete`** — todas las operaciones confirmadas y los datos durables en disco.
- **`onerror`** — alguna operación falló; la transacción se aborta automáticamente.
- **`onabort`** — la transacción fue abortada (por error o por llamada explícita a `tx.abort()`).

> [!warning]
> Una transacción se cierra automáticamente en el siguiente ciclo del event loop después de que no queden operaciones `IDBRequest` pendientes. No se puede reutilizar una transacción ya confirmada ni abortada. Con la API nativa, iniciar operaciones asíncronas (como `fetch`) dentro de una transacción la cierra antes de que la respuesta llegue — usar `idb` y `await` de forma secuencial dentro del bloque de transacción, o separar la obtención de datos de la escritura.

> [!tip]
> Para leer datos sin modificarlos, usar siempre `"readonly"`. Las transacciones de solo lectura pueden ejecutarse concurrentemente; las `"readwrite"` sobre el mismo store se serializan. Minimizar el scope de stores en cada transacción (`db.transaction(["storeA"], "readwrite")` en lugar de incluir todos) reduce la contención.

## Notas relacionadas

- [[01 Conceptos (stores, índices)|Conceptos: stores, índices y versiones]] — apertura de la BD, object stores, keyPath y `upgradeneeded`
- [[index|IndexedDB]] — visión general y comparativa con Web Storage
- [[../04 Cache API|Cache API]] — almacenamiento de pares Request/Response (distinto de datos estructurados)
