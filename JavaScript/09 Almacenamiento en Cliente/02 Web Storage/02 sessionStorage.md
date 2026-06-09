---
title: sessionStorage — almacenamiento por pestaña
aliases:
  - sessionStorage
  - window.sessionStorage
tags:
  - javascript
  - api/objeto
  - almacenamiento
objeto: window
tipo: propiedad
draft: false
---

# `sessionStorage`

> [!definicion]
> `window.sessionStorage` comparte exactamente la misma API que `localStorage` pero con un scope diferente: los datos se eliminan al cerrar la **pestaña** (o el contexto de navegación). No se comparte entre pestañas, ni siquiera del mismo origen — cada pestaña tiene su propio `sessionStorage` aislado.

```js
// Guardar estado de un formulario multi-paso
sessionStorage.setItem("paso-actual", "3");
sessionStorage.setItem("form-datos", JSON.stringify({ nombre: "Ana", email: "ana@mail.com" }));

// Leer al volver a la página (misma pestaña)
const paso = sessionStorage.getItem("paso-actual"); // "3"

// Al completar el formulario: limpiar
sessionStorage.clear();
```

## Mismos métodos que localStorage

`sessionStorage` implementa la misma interfaz `Storage`. Todos los métodos son idénticos:

```js
sessionStorage.setItem(key, value)
sessionStorage.getItem(key)       // string | null
sessionStorage.removeItem(key)
sessionStorage.clear()
sessionStorage.key(index)
sessionStorage.length
```

La única diferencia es el comportamiento de persistencia y aislamiento.

## Caso de uso: wizard o formulario multi-paso

`sessionStorage` es ideal para formularios de varios pasos donde los datos intermedios no deben persistir entre sesiones ni filtrarse a otras pestañas:

```js
const WIZARD_KEY = "wizard-compra";

function guardarPaso(numeroPaso, datos) {
  const estado = JSON.parse(sessionStorage.getItem(WIZARD_KEY) ?? "{}");
  estado[`paso${numeroPaso}`] = datos;
  sessionStorage.setItem(WIZARD_KEY, JSON.stringify(estado));
}

function recuperarPaso(numeroPaso) {
  const estado = JSON.parse(sessionStorage.getItem(WIZARD_KEY) ?? "{}");
  return estado[`paso${numeroPaso}`] ?? null;
}

function limpiarWizard() {
  sessionStorage.removeItem(WIZARD_KEY);
}

// Guardar al avanzar
guardarPaso(1, { nombre: "Ana", apellido: "García" });
guardarPaso(2, { direccion: "Calle Mayor 1", ciudad: "Madrid" });

// Recuperar al volver atrás
recuperarPaso(1); // { nombre: "Ana", apellido: "García" }

// Limpiar al completar o cancelar
limpiarWizard();
```

## Comportamiento al duplicar una pestaña

`sessionStorage` se **copia** cuando el usuario abre una nueva pestaña duplicando la existente (Ctrl+T en Firefox/Safari, o "Duplicar pestaña" en Chrome). La nueva pestaña hereda el `sessionStorage` de la original, pero desde ese momento son independientes — los cambios en una no afectan a la otra:

```js
// Pestaña original: sessionStorage.getItem("x") = "original"

// Usuario duplica la pestaña:
// Nueva pestaña hereda: sessionStorage.getItem("x") = "original"

// En la nueva pestaña:
sessionStorage.setItem("x", "modificado");

// En la pestaña original: sessionStorage.getItem("x") = "original"  (no cambió)
```

Abrir una URL directamente en una nueva pestaña crea un `sessionStorage` vacío — no hay herencia.

## Comparativa: localStorage vs sessionStorage vs Cookies

| Característica | localStorage | sessionStorage | Cookies |
|---|---|---|---|
| Persistencia | Indefinida (hasta clear/removeItem) | Hasta cerrar la pestaña | Configurable (sesión o fecha fija) |
| Scope de pestaña | Compartido entre todas las pestañas del origen | Solo la pestaña actual | Compartido entre pestañas |
| Compartido entre pestañas | Sí | No | Sí |
| Enviado al servidor | No | No | Sí, automáticamente |
| Límite de tamaño | ~5-10MB | ~5-10MB | ~4KB |
| Acceso desde JS | Sí | Sí | Solo sin HttpOnly |
| Disponible en SW | No | No | Solo CookieStore |
| Dispara evento storage | Sí (en otras pestañas) | No (no es compartido) | No |

## Cómo funciona por dentro

El "contexto de navegación" al que está vinculado `sessionStorage` es el concepto del browser equivalente a una pestaña con su historial. Cuando la pestaña se cierra, el motor de JS destruye el contexto de navegación y con él el `sessionStorage`. A diferencia de `localStorage`, los datos de `sessionStorage` típicamente solo residen en memoria (no se persisten en disco entre sesiones), aunque esto depende de la implementación del navegador.

El aislamiento entre pestañas es estricto — incluso si dos pestañas tienen exactamente el mismo origen, no pueden leer el `sessionStorage` de la otra. Para comunicación entre pestañas hay que usar el evento `storage` de `localStorage` o `BroadcastChannel`.

> [!tip]
> Para guardar el scroll position o el estado de la UI entre navegaciones dentro de la misma pestaña (hacia atrás y adelante), `sessionStorage` es la opción correcta: los datos persisten mientras la pestaña esté abierta pero no contaminan otras sesiones.

> [!warning]
> No asumir que `sessionStorage` se limpia al cerrar solo la pestaña en todos los entornos. En algunos navegadores móviles o PWAs, el concepto de "cerrar pestaña" puede ser ambiguo cuando la app queda en segundo plano. Para datos que **nunca** deben persistir, complementar `sessionStorage` con `sessionStorage.clear()` explícito en el evento `beforeunload` si la garantía es crítica.

## Notas relacionadas

- [[01 localStorage|localStorage]] — misma API, persistencia indefinida y compartida entre pestañas
- [[03 Evento storage|Evento storage]] — por qué sessionStorage no dispara el evento storage
- [[index|Web Storage]] — visión general y comparativa
