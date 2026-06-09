---
title: 09 Almacenamiento en Cliente — visión general
aliases:
  - Almacenamiento en Cliente
  - Client Storage
tags:
  - javascript
  - api/web
  - almacenamiento
draft: false
---

# 09 Almacenamiento en Cliente

> [!definicion]
> El navegador expone cuatro mecanismos de almacenamiento persistente en el cliente: **Cookies**, **Web Storage** (`localStorage`/`sessionStorage`), **IndexedDB** y **Cache API**. Cada uno responde a un caso de uso distinto determinado por su capacidad, sincronía, tipo de datos que acepta y si los datos viajan al servidor con cada petición HTTP.

```js
// Snapshot de los mecanismos disponibles en el origen actual
const [idbEstimacion, cacheKeys] = await Promise.all([
  navigator.storage.estimate(),
  caches.keys()
]);
console.log(idbEstimacion); // { quota: 2147483648, usage: 1048576 }
console.log(cacheKeys);     // ["shell-v1", "dynamic-v1"]
```

## Mecanismos disponibles

| Mecanismo | Capacidad | Persistencia | Sincronía | Enviado al servidor | Cuándo usar |
|---|---|---|---|---|---|
| Cookies | ~4 KB/cookie, ~50/dominio | Configurable (expires/max-age) | Síncrono | Sí, automáticamente | Sesiones, autenticación, preferencias que el servidor debe leer |
| `localStorage` | ~5-10 MB | Sin expiración | Síncrono | No | Preferencias de UI, estado de app pequeño, solo strings |
| `sessionStorage` | ~5-10 MB | Hasta cerrar la pestaña | Síncrono | No | Datos de sesión de pestaña, flujos multi-paso |
| IndexedDB | Decenas-cientos de MB | Sin expiración | Asíncrono | No | Datos estructurados grandes, blobs, cachés de datos offline |
| Cache API | Sujeto a cuota del origen | Sin expiración (gestión manual) | Asíncrono | No | Assets estáticos para PWA, estrategias offline con Service Worker |

## Contenido de esta sección

- [[01 Cookies/index|Cookies]] — `document.cookie`, atributos de seguridad (`HttpOnly`, `SameSite`, `Secure`), CookieStore API.
- [[02 Web Storage/index|Web Storage]] — `localStorage` y `sessionStorage`: API síncrona clave-valor, evento `storage`.
- [[03 IndexedDB/index|IndexedDB]] — base de datos NoSQL orientada a objetos: stores, índices, transacciones ACID, librería `idb`.
- [[04 Cache API|Cache API]] — pares Request/Response para Service Workers: precacheo, estrategias Cache First / Network First / Stale-While-Revalidate.

## Criterios de elección

El mecanismo adecuado se determina por tres preguntas:

1. **¿El servidor necesita leer el dato?** → Cookie con `HttpOnly; Secure; SameSite=Lax` establecida desde el backend.
2. **¿Son datos de estructura compleja o volumen grande?** → IndexedDB (con la librería `idb`).
3. **¿Son assets estáticos para soporte offline / PWA?** → Cache API desde un Service Worker.

Para todo lo demás (preferencias de UI simples, estado de formulario, flags de sesión de pestaña), `localStorage` o `sessionStorage` son suficientes y tienen la API más sencilla.

> [!tip]
> La cuota disponible para IndexedDB y Cache API se puede consultar con `navigator.storage.estimate()`. En contextos donde la persistencia es crítica (apps offline), solicitar almacenamiento persistente con `navigator.storage.persist()` previene que el navegador elimine los datos bajo presión de memoria.

> [!warning]
> `localStorage` y `sessionStorage` son síncronos y bloquean el hilo principal. Operaciones frecuentes o con datos grandes degradan la interactividad. Para cualquier caso no trivial con datos estructurados, IndexedDB con `idb` es la alternativa correcta.

## Notas relacionadas

- [[../07 Programación Asíncrona/index|Programación Asíncrona]] — Promises y async/await, necesarios para IndexedDB y Cache API
- [[../08 Comunicación con APIs/index|Comunicación con APIs]] — fetch y la Request/Response que la Cache API almacena
