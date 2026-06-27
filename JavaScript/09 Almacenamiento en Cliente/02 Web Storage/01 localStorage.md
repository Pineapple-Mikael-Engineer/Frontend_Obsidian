---
title: localStorage — almacenamiento persistente por origen
aliases:
  - localStorage
  - window.localStorage
tags:
  - javascript
  - api/objeto
  - almacenamiento
objeto: window
tipo: propiedad
draft: false
order: 1
---

# `localStorage`

> [!definicion]
> `window.localStorage` es un objeto `Storage` que almacena pares clave-valor como strings de forma **persistente por origen**. Los datos sobreviven al cerrar el navegador, reiniciar el sistema y actualizaciones de la app. El storage es compartido entre todas las pestañas y ventanas del mismo origen (`protocolo + dominio + puerto`).

```js
// Guardar
localStorage.setItem("tema", "oscuro");
localStorage.setItem("idioma", "es");

// Leer
localStorage.getItem("tema");  // "oscuro"
localStorage.getItem("noExiste"); // null

// Eliminar una entrada
localStorage.removeItem("idioma");

// Limpiar todo el storage del origen
localStorage.clear();

// Número de entradas
localStorage.length; // 1
```

## Métodos de la API

| Método / Propiedad | Firma | Retorna | Descripción |
|---|---|---|---|
| `setItem` | `(key: string, value: string)` | `void` | Guarda o actualiza el par; convierte value a string automáticamente |
| `getItem` | `(key: string)` | `string \| null` | Devuelve el valor o `null` si la clave no existe |
| `removeItem` | `(key: string)` | `void` | Elimina la entrada; no lanza si la clave no existe |
| `clear` | `()` | `void` | Elimina todas las entradas del origen |
| `key` | `(index: number)` | `string \| null` | Nombre de la clave en esa posición (orden no garantizado) |
| `length` | propiedad | `number` | Número de entradas almacenadas |

## Solo almacena strings

`localStorage` convierte cualquier valor a string con `toString()`. Para objetos hay que serializar explícitamente:

```js
// MAL: guarda "[object Object]"
localStorage.setItem("usuario", { nombre: "Ana" });

// BIEN: serializar con JSON
localStorage.setItem("usuario", JSON.stringify({ nombre: "Ana", rol: "admin" }));

// Leer y deserializar
const usuario = JSON.parse(localStorage.getItem("usuario") ?? "null");
usuario?.nombre; // "Ana"
```

## Helper tipado para objetos

```js
function lsSet(clave, valor) {
  localStorage.setItem(clave, JSON.stringify(valor));
}

function lsGet(clave, fallback = null) {
  try {
    const raw = localStorage.getItem(clave);
    return raw !== null ? JSON.parse(raw) : fallback;
  } catch {
    return fallback;
  }
}

function lsDel(clave) {
  localStorage.removeItem(clave);
}

// Uso
lsSet("config", { tema: "oscuro", fuente: 16 });
lsGet("config");         // { tema: "oscuro", fuente: 16 }
lsGet("noExiste", {});   // {}
```

## Receta: preferencias de tema (claro/oscuro)

```js
const TEMA_KEY = "preferencia-tema";

function getTema() {
  return lsGet(TEMA_KEY) ?? "claro";
}

function setTema(tema) {
  lsSet(TEMA_KEY, tema);
  document.documentElement.dataset.tema = tema;
}

function toggleTema() {
  setTema(getTema() === "claro" ? "oscuro" : "claro");
}

// Al cargar la página: aplicar preferencia guardada
document.documentElement.dataset.tema = getTema();
```

## Manejar QuotaExceededError

Cuando el origen supera el límite de almacenamiento (~5MB, varía por navegador), `setItem` lanza `DOMException` con nombre `"QuotaExceededError"`:

```js
function lsSetSafe(clave, valor) {
  try {
    localStorage.setItem(clave, JSON.stringify(valor));
    return true;
  } catch (e) {
    if (e instanceof DOMException && e.name === "QuotaExceededError") {
      console.warn("localStorage lleno — limpiando entradas antiguas");
      // Estrategia: limpiar entradas marcadas como prescindibles
      localStorage.removeItem("cache-busquedas");
      try {
        localStorage.setItem(clave, JSON.stringify(valor));
        return true;
      } catch {
        return false;
      }
    }
    throw e;
  }
}
```

## Modo incógnito y restricciones

En navegación privada, `localStorage` suele estar disponible pero se limpia al cerrar la ventana de incógnito (comportamiento similar a `sessionStorage`). En algunos contextos de extensiones de navegador o iframes con `sandbox`, `localStorage` puede estar deshabilitado completamente:

```js
function lsDisponible() {
  try {
    const test = "__test__";
    localStorage.setItem(test, "1");
    localStorage.removeItem(test);
    return true;
  } catch {
    return false;
  }
}

if (!lsDisponible()) {
  // Fallback: estado en memoria
}
```

## Iterar sobre todas las entradas

```js
// Iterar por índice
for (let i = 0; i < localStorage.length; i++) {
  const clave = localStorage.key(i);
  const valor = localStorage.getItem(clave);
  console.log(clave, valor);
}

// Con Object.entries (crea snapshot — no usa la API de Storage)
const entradas = Object.entries(localStorage);
```

## Cómo funciona por dentro

`localStorage` es implementado por el navegador como un almacén persistente por origen, típicamente en una base de datos SQLite o similar en el perfil del usuario. Las operaciones son síncronas y bloquean el hilo principal porque el acceso al disco se hace en el hilo del motor JS (no en un worker). Esto es una limitación de diseño de la API: fue especificada en una época anterior a los Web Workers. Los datos se leen en memoria al inicio de la sesión y se escriben en disco en cada `setItem`, lo que explica por qué las escrituras frecuentes de grandes strings pueden causar jank perceptible.

> [!tip]
> Para datos JSON, el patrón `JSON.parse(localStorage.getItem(key) ?? "null")` es preferible a comprobar `!== null` por separado: si `getItem` devuelve `null`, `JSON.parse("null")` retorna `null` sin lanzar, y se puede usar el operador de coalescencia nula para el fallback.

> [!warning]
> `localStorage` es accesible por **cualquier script del mismo origen**, incluyendo scripts de terceros (anuncios, analytics, CDNs comprometidos). No almacenar tokens de autenticación ni información sensible en `localStorage` — usar cookies `HttpOnly` para esos datos.

## Notas relacionadas

- [[02 sessionStorage|sessionStorage]] — misma API, scope limitado a la pestaña
- [[03 Evento storage|Evento storage]] — detectar cambios en localStorage desde otras pestañas
- [[index|Web Storage]] — visión general y comparativa
