---
title: Web Storage — localStorage y sessionStorage
aliases:
  - Web Storage
  - localStorage
  - sessionStorage
tags:
  - javascript
  - api/web
  - almacenamiento
draft: false
order: 2
---

# Web Storage

> [!definicion]
> **Web Storage** es la API del navegador que agrupa `localStorage` y `sessionStorage`: dos objetos síncronos para almacenar pares clave-valor como strings, aislados por origen. Límite: ~5-10MB por origen (frente a ~4KB de las cookies). Los datos **no se envían al servidor** automáticamente, lo que los hace adecuados para estado puramente del cliente.

```js
// localStorage — persiste indefinidamente
localStorage.setItem("tema", "oscuro");
localStorage.getItem("tema"); // "oscuro"

// sessionStorage — dura mientras la pestaña esté abierta
sessionStorage.setItem("paso", "2");
sessionStorage.getItem("paso"); // "2"
```

Ambas APIs comparten exactamente los mismos métodos. La diferencia está en el **scope temporal y de pestaña**: `localStorage` es persistente y compartida entre pestañas del mismo origen; `sessionStorage` es temporal y privada a la pestaña.

## Bloques de esta sección

- [[01 localStorage|localStorage]] — almacenamiento persistente por origen, métodos, límites y recetas.
- [[02 sessionStorage|sessionStorage]] — almacenamiento por pestaña, diferencias con localStorage y casos de uso.
- [[03 Evento storage|Evento storage]] — sincronización entre pestañas y alternativas modernas.

## Métodos de la API (comunes a ambas)

| Método / Propiedad | Descripción |
|---|---|
| `setItem(key, value)` | Guarda o actualiza el par clave-valor (ambos deben ser strings) |
| `getItem(key)` | Devuelve el valor o `null` si la clave no existe |
| `removeItem(key)` | Elimina la entrada de la clave indicada |
| `clear()` | Elimina todas las entradas del storage |
| `key(index)` | Devuelve el nombre de la clave en la posición indicada |
| `length` | Número de entradas almacenadas |

## Comparativa general

| Característica | localStorage | sessionStorage | Cookies |
|---|---|---|---|
| Persistencia | Indefinida | Hasta cerrar la pestaña | Configurable |
| Scope de pestaña | Compartido entre pestañas | Solo la pestaña actual | Compartido entre pestañas |
| Enviado al servidor | No | No | Sí, automáticamente |
| Límite | ~5-10MB | ~5-10MB | ~4KB |
| Acceso desde JS | Sí | Sí | Solo sin HttpOnly |

> [!tip]
> Para preferencias de usuario (tema, idioma, layout) que deben sobrevivir al cierre del browser, `localStorage` es la opción natural. Para datos de un wizard o formulario multi-paso que no deben persistir entre sesiones, `sessionStorage` es más seguro.

> [!warning]
> Web Storage es **síncrono** — operaciones de lectura y escritura bloquean el hilo principal. Para grandes volúmenes de datos o acceso frecuente de alta frecuencia, `IndexedDB` es la alternativa asíncrona. No almacenar datos sensibles (tokens de autenticación, PII) en Web Storage: son accesibles por cualquier script del mismo origen.

## Notas relacionadas

- [[../01 Cookies/index|Cookies]] — alternativa con transmisión al servidor y configuración de seguridad
- [[../03 IndexedDB/index|IndexedDB]] — almacenamiento estructurado y asíncrono para grandes volúmenes
