---
title: "IndexedDB — Conceptos: stores, índices y versiones"
aliases:
  - IDB stores
  - IndexedDB object store
  - IndexedDB índice
  - IndexedDB versión
tags:
  - javascript
  - api/web
  - almacenamiento
objeto: indexedDB
tipo: concepto
asincrono: true
draft: false
order: 1
---

# IndexedDB — Conceptos: stores, índices y versiones

> [!definicion]
> Una **base de datos IndexedDB** queda identificada por un nombre de string y un número de versión entero. Contiene uno o más **object stores** (equivalentes a tablas), cada uno con una clave primaria y, opcionalmente, índices secundarios. Los cambios estructurales (crear o eliminar stores e índices) solo pueden realizarse dentro del manejador del evento `upgradeneeded`, que se dispara cuando se abre la BD con una versión mayor a la almacenada.

```js
const req = indexedDB.open("miApp", 1);

req.onupgradeneeded = e => {
  const db = e.target.result;                                      // IDBDatabase
  const store = db.createObjectStore("usuarios", { keyPath: "id" });
  store.createIndex("email", "email", { unique: true });
  store.createIndex("apellido", "apellido", { unique: false });
};

req.onsuccess   = e => { const db = e.target.result; /* usar db */ };
req.onerror     = e => { console.error(e.target.error); };
```

## Conceptos clave

| Concepto | Equivalente relacional | Descripción |
|---|---|---|
| Database | Base de datos | Contenedor con nombre y versión |
| Object Store | Tabla | Colección de objetos JS serializados con structured clone |
| Key (clave) | Primary key | Identifica unívocamente cada registro |
| Index (índice) | Índice secundario | Permite buscar por propiedades distintas a la clave primaria |
| Version | Schema version | Entero; cambiar la versión dispara `upgradeneeded` |
| Cursor | Iterador | Recorre registros de un store o índice en orden de clave |

## Apertura de la base de datos

`indexedDB.open(nombre, versión)` devuelve un `IDBOpenDBRequest`. Los eventos que puede disparar son:

- **`onsuccess`** — la BD se abrió en la versión solicitada; `e.target.result` es el objeto `IDBDatabase`.
- **`onerror`** — no se pudo abrir (permisos, cuota, etc.).
- **`onupgradeneeded`** — la versión solicitada es mayor que la almacenada (o la BD no existe). Es el **único lugar** donde se pueden crear o borrar stores e índices.
- **`onblocked`** — otra pestaña tiene la BD abierta en una versión anterior; la actualización queda bloqueada hasta que se cierren esas conexiones.

## Object Stores y claves

`db.createObjectStore(nombre, opciones)` — solo válido dentro de `upgradeneeded`.

```js
// In-line key: la propiedad "id" del objeto es la clave primaria
db.createObjectStore("productos", { keyPath: "id" });

// Auto-increment: clave numérica generada automáticamente
db.createObjectStore("logs", { autoIncrement: true });

// Out-of-line key: clave separada del objeto, se proporciona al escribir
db.createObjectStore("configs");
// Al escribir: store.add(valorObjeto, claveExplicita)
```

- **`keyPath`** — propiedad del objeto (o ruta de puntos `"direccion.cp"`) que actúa como clave primaria. Si el objeto no tiene esa propiedad, `add`/`put` lanzan un error.
- **`autoIncrement`** — genera una clave entera incremental; compatible con `keyPath` (rellena la propiedad automáticamente).
- **Out-of-line keys** — sin `keyPath` ni `autoIncrement`; la clave se pasa como segundo argumento en `add(obj, key)` y `put(obj, key)`.

## Índices

Un índice permite recuperar registros por una propiedad distinta a la clave primaria, sin iterar todo el store.

```js
req.onupgradeneeded = e => {
  const db    = e.target.result;
  const store = db.createObjectStore("usuarios", { keyPath: "id" });

  store.createIndex("por-email",    "email",    { unique: true  });
  store.createIndex("por-apellido", "apellido", { unique: false });
  // Índice compuesto (ruta de array de propiedades)
  store.createIndex("por-pais-ciudad", ["pais", "ciudad"], { unique: false });
};
```

`store.createIndex(nombre, keyPath, { unique, multiEntry })`:

- **`unique: true`** — no permite dos registros con el mismo valor en ese campo.
- **`multiEntry: true`** — si el campo es un array, crea una entrada de índice por cada elemento.

Para usar un índice desde una transacción: `store.index("por-email").get("ana@example.com")`.

## Ciclo de vida de la versión

El esquema de IndexedDB sigue un modelo de migraciones basado en el número de versión:

```js
// Versión 2: añadir un store nuevo y un índice al existente
const req = indexedDB.open("miApp", 2);

req.onupgradeneeded = e => {
  const db       = e.target.result;
  const oldVer   = e.oldVersion; // 1 en este caso

  if (oldVer < 1) {
    // Creaciones de la versión 1
    db.createObjectStore("usuarios", { keyPath: "id" });
  }
  if (oldVer < 2) {
    // Migraciones de la versión 1 → 2
    const store = e.target.transaction.objectStore("usuarios");
    store.createIndex("por-ciudad", "ciudad", { unique: false });
    db.createObjectStore("pedidos", { keyPath: "pedidoId", autoIncrement: true });
  }
};
```

> [!warning]
> Los stores e índices **no se pueden modificar** fuera de `upgradeneeded`. Si se necesita cambiar el `keyPath` de un store existente, hay que eliminarlo (`db.deleteObjectStore`) y recrearlo, lo que implica migrar los datos manualmente dentro del mismo manejador de `upgradeneeded`.

## Cómo funciona por dentro

IndexedDB usa el algoritmo **structured clone** para serializar los valores: admite la mayoría de tipos JS (objetos anidados, arrays, `Date`, `Map`, `Set`, `ArrayBuffer`, `Blob`, `File`), pero no funciones, `undefined` como valor de propiedad de objeto, ni nodos del DOM. La BD persiste en disco gestionada por el navegador; el origen (protocol + host + port) es el namespace de aislamiento.

> [!tip]
> Para entornos de producción, la librería `idb` (Jake Archibald) convierte toda la API en Promises y elimina el boilerplate de eventos. El snippet con `idb` es la forma recomendada; la API nativa solo es necesaria cuando no se pueden usar dependencias externas. Ver [[02 Transacciones|Transacciones]] para el uso con `idb`.

## Notas relacionadas

- [[02 Transacciones|Transacciones y operaciones CRUD]] — modos de transacción, `add`/`put`/`get`/`delete`, cursores, búsqueda por índice
- [[index|IndexedDB]] — visión general y comparativa con Web Storage
