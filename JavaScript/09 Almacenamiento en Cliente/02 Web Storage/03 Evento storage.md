---
title: Evento storage — sincronización entre pestañas
aliases:
  - evento storage
  - StorageEvent
  - storage event
tags:
  - javascript
  - api/evento
  - almacenamiento
draft: false
order: 3
---

# Evento `storage`

> [!definicion]
> El evento `storage` se dispara en las **otras** pestañas del mismo origen cuando `localStorage` cambia — nunca en la pestaña que realizó el cambio. Permite sincronizar estado entre pestañas: un cambio en `localStorage` hecho en la pestaña A notifica a las pestañas B, C, etc. `sessionStorage` no dispara este evento porque no es compartido entre pestañas.

```js
window.addEventListener("storage", e => {
  console.log(e.key);         // clave modificada (null si fue clear())
  console.log(e.oldValue);    // valor anterior (null si era nueva o si fue clear())
  console.log(e.newValue);    // nuevo valor (null si fue removeItem() o clear())
  console.log(e.url);         // URL de la pestaña que hizo el cambio
  console.log(e.storageArea); // referencia al objeto Storage (localStorage)
});
```

## Propiedades del StorageEvent

| Propiedad | Tipo | Descripción |
|---|---|---|
| `key` | `string \| null` | Clave modificada; `null` si se llamó a `clear()` |
| `oldValue` | `string \| null` | Valor anterior de la clave; `null` si la clave era nueva o se llamó `clear()` |
| `newValue` | `string \| null` | Nuevo valor; `null` si se llamó `removeItem()` o `clear()` |
| `url` | `string` | URL del documento en la pestaña que realizó el cambio |
| `storageArea` | `Storage \| null` | Referencia al objeto `localStorage` o `sessionStorage` que cambió |

## Caso de uso: logout sincronizado entre pestañas

El uso más común del evento `storage` es propagar un logout o cambio de sesión a todas las pestañas abiertas:

```js
// En la pestaña que hace logout:
function logout() {
  fetch("/api/logout", { method: "POST" }).then(() => {
    localStorage.setItem("sesion-cerrada", Date.now().toString());
    localStorage.removeItem("sesion-cerrada"); // dispara el evento en otras pestañas
    window.location.href = "/login";
  });
}

// En todas las pestañas (listener activo):
window.addEventListener("storage", e => {
  if (e.key === "sesion-cerrada") {
    // Esta pestaña fue notificada de un logout en otra pestaña
    window.location.href = "/login";
  }
});
```

## Canal de mensajes entre pestañas con storage

Se puede usar `localStorage` + el evento `storage` como canal de mensajes unidireccional entre pestañas:

```js
// Emisor (cualquier pestaña)
function emitirMensaje(canal, datos) {
  const mensaje = JSON.stringify({ datos, ts: Date.now() });
  localStorage.setItem(`msg:${canal}`, mensaje);
  // Limpiar inmediatamente: el evento storage se disparó en las otras pestañas
  localStorage.removeItem(`msg:${canal}`);
}

// Receptor (otras pestañas)
window.addEventListener("storage", e => {
  if (e.key?.startsWith("msg:") && e.newValue === null && e.oldValue) {
    const canal = e.key.slice(4);
    const { datos } = JSON.parse(e.oldValue);
    console.log(`Mensaje en canal "${canal}":`, datos);
  }
});

// Uso
emitirMensaje("carrito", { accion: "añadir", productoId: 42 });
```

## sessionStorage no dispara el evento

`sessionStorage` no dispara el evento `storage` porque no es compartido entre pestañas. Cada pestaña tiene su propio `sessionStorage` aislado, por lo que no hay a quién notificar:

```js
// Esto NO dispara el evento "storage" en ninguna pestaña
sessionStorage.setItem("clave", "valor");
```

Para comunicación desde `sessionStorage` entre pestañas habría que copiar los datos a `localStorage` primero.

## Alternativa moderna: BroadcastChannel

`BroadcastChannel` es la API diseñada específicamente para comunicación entre pestañas del mismo origen. Ofrece una interfaz más explícita y bidireccional que el truco con `localStorage`:

```js
// En todas las pestañas
const canal = new BroadcastChannel("app-sync");

// Emitir mensaje
canal.postMessage({ tipo: "logout", usuario: "ana" });

// Recibir mensaje
canal.addEventListener("message", e => {
  console.log(e.data); // { tipo: "logout", usuario: "ana" }

  if (e.data.tipo === "logout") {
    window.location.href = "/login";
  }
});

// Cerrar el canal cuando ya no se necesite
canal.close();
```

| Característica | Evento storage | BroadcastChannel |
|---|---|---|
| Dirección | Unidireccional (cambio → notificación) | Bidireccional explícito |
| Requiere modificar storage | Sí | No |
| La pestaña emisora recibe | No | No (a menos que sea el mismo canal) |
| Soporte | Universal | Chrome 54+, Firefox 38+, Safari 15.4+ |
| Disponible en SW | No | Sí |
| Puede enviar objetos | Solo strings (vía JSON) | Cualquier objeto serializable (structured clone) |

## Cómo funciona por dentro

El evento `storage` es generado por el navegador a nivel del motor de storage, no por JS. Cuando una pestaña llama a `localStorage.setItem()`, el motor actualiza el almacén compartido del origen y luego recorre todos los contextos de navegación del mismo origen que tienen un listener de `storage` registrado, disparando el evento en cada uno excepto en el que originó el cambio. Este mecanismo usa el sistema de IPC (Inter-Process Communication) del navegador entre los procesos de renderer de cada pestaña.

> [!tip]
> Para sincronización entre pestañas en aplicaciones nuevas, preferir `BroadcastChannel` sobre el evento `storage`. `BroadcastChannel` tiene semántica más clara, no requiere escribir y borrar en `localStorage`, y soporta cualquier objeto serializable. El evento `storage` es útil cuando ya se está usando `localStorage` para persistencia y la sincronización es un efecto secundario deseable.

> [!warning]
> El evento `storage` **no se dispara en la pestaña que hace el cambio**. Si una lógica de actualización de UI depende de este evento, la pestaña emisora debe actualizar su propia UI directamente después de modificar `localStorage`, sin esperar al evento.

## Notas relacionadas

- [[01 localStorage|localStorage]] — el storage que dispara este evento
- [[02 sessionStorage|sessionStorage]] — por qué no dispara el evento storage
- [[index|Web Storage]] — visión general de localStorage y sessionStorage
