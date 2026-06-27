---
title: window.open y window.close
aliases:
  - window.open
  - window.close
  - window.opener
  - postMessage
  - popup javascript
tags:
  - javascript
  - api/objeto
  - browser
draft: false
order: 2
---

# `window.open` y `window.close`

> [!definicion]
> `window.open(url, target, features)` abre una URL en una nueva pestaña o ventana popup y devuelve una referencia al nuevo objeto `window`. `window.close()` cierra la ventana actual, pero solo funciona si la ventana fue abierta por script. Ambas forman la base para flujos de OAuth, visualizadores y comunicación entre ventanas.

```js
// Abrir una nueva pestaña (comportamiento por defecto en browsers modernos)
const nuevaPestana = window.open("https://ejemplo.com", "_blank");

// Abrir un popup de 500×400 en una posición específica (seguro con noopener)
const popup = window.open(
  "https://auth.ejemplo.com/login",
  "authPopup",
  "width=500,height=600,left=100,top=100,noopener,noreferrer"
);

// Comprobar si fue bloqueado
if (!popup) {
  alert("El popup fue bloqueado. Permítelo en la barra de dirección.");
}

// Cerrar la ventana actual (si fue abierta por script)
window.close();
```

## Parámetros de `window.open`

| Parámetro | Tipo | Descripción |
|---|---|---|
| `url` | string | URL a cargar en la nueva ventana. `""` abre `about:blank`. |
| `target` | string | Nombre del contexto de navegación destino |
| `features` | string | String de características del popup, separadas por coma |

### Valores de `target`

| Valor | Efecto |
|---|---|
| `"_blank"` | Nueva pestaña (o ventana, si `features` define dimensiones) |
| `"_self"` | Misma pestaña/ventana actual |
| `"_parent"` | Frame padre |
| `"_top"` | Ventana raíz (sale de todos los frames) |
| nombre personalizado | Reutiliza una ventana con ese nombre si ya existe |

### Opciones de `features` más comunes

| Opción | Descripción |
|---|---|
| `width=N` | Ancho de la ventana popup en píxeles |
| `height=N` | Alto del popup |
| `left=N` | Posición horizontal en la pantalla |
| `top=N` | Posición vertical en la pantalla |
| `noopener` | Hace `window.opener = null` en la ventana hija (seguridad) |
| `noreferrer` | No envía `Referer` header — implica `noopener` |
| `menubar=no` | Oculta la barra de menú (solo popups, ignorado en pestañas) |
| `toolbar=no` | Oculta la barra de herramientas |
| `scrollbars=yes` | Muestra scrollbars en el popup |
| `resizable=yes` | Permite redimensionar el popup |

En browsers modernos, si se especifican `width` y `height` sin `noopener`, el browser puede decidir abrir una pestaña en lugar de un popup. Solo abre un popup real si la llamada es el resultado directo de una interacción del usuario (click, tap).

## `window.opener`

La ventana abierta con `window.open` tiene `window.opener` apuntando a la ventana padre. Esto permite comunicación bidireccional, pero también es un vector de seguridad: si una ventana hija puede modificar `opener.location`, puede redirigir la pestaña padre a una URL maliciosa (**tabnapping**).

```js
// En la ventana hija: acceder al opener
if (window.opener) {
  window.opener.document.title = "Modificado desde la ventana hija";
}

// Prevenir tabnapping: usar noopener en window.open
const popup = window.open("https://sitio-externo.com", "_blank", "noopener,noreferrer");
// Con noopener: popup.opener === null — el popup no puede tocar la ventana padre
```

## `window.postMessage` — comunicación segura entre ventanas

Cuando `noopener` está activo (o cuando las ventanas son de orígenes distintos), `window.postMessage` es el canal de comunicación seguro.

```js
// Ventana padre: enviar mensaje al popup
popup.postMessage({ tipo: "token", valor: "abc123" }, "https://auth.ejemplo.com");

// Popup (receptor): recibir el mensaje
window.addEventListener("message", (event) => {
  // SIEMPRE verificar el origen
  if (event.origin !== "https://mi-app.com") return;

  const { tipo, valor } = event.data;
  if (tipo === "token") {
    procesarToken(valor);
    window.close(); // Cerrar el popup tras procesar
  }
});
```

## Cómo funciona por dentro

Cuando se llama a `window.open`, el browser crea un nuevo contexto de navegación (un nuevo proceso en Chromium con aislamiento de sitio, o un nuevo hilo de rendering). El string `features` es una herencia de los años 90 cuando los popups eran ventanas del OS. Hoy, browsers modernos convierten casi todos los `window.open` en pestañas del mismo proceso si no hay razón para aislar. La excepción son los popups con dimensiones explícitas lanzados desde un click del usuario.

Los popups no iniciados por interacción del usuario son bloqueados por el popup blocker del browser. La referencia devuelta será `null` en ese caso. La heurística es: si `window.open` se llama dentro del handler de un evento de usuario (click, keydown), se permite; si se llama en un timeout o de forma asíncrona separada del evento, se bloquea.

## Receta: popup de OAuth seguro

```js
async function loginConOAuth(proveedor) {
  const anchoPop = 500;
  const altoPop = 650;
  const left = (screen.width - anchoPop) / 2;
  const top = (screen.height - altoPop) / 2;

  const popup = window.open(
    `/auth/${proveedor}`,
    "oauth_popup",
    `width=${anchoPop},height=${altoPop},left=${left},top=${top},noopener`
  );

  if (!popup) {
    throw new Error("Popup bloqueado — pide al usuario que lo permita");
  }

  return new Promise((resolve, reject) => {
    window.addEventListener("message", function handler(event) {
      if (event.origin !== window.location.origin) return;
      if (event.data?.tipo !== "oauth_callback") return;

      window.removeEventListener("message", handler);

      if (event.data.error) {
        reject(new Error(event.data.error));
      } else {
        resolve(event.data.token);
      }
    });

    // Detectar si el usuario cerró el popup manualmente
    const intervalo = setInterval(() => {
      if (popup.closed) {
        clearInterval(intervalo);
        reject(new Error("El usuario cerró el popup"));
      }
    }, 500);
  });
}
```

> [!tip]
> Para centrar un popup en la pantalla del usuario, calcular `left = (screen.width - width) / 2` y `top = (screen.height - height) / 2`. En setups multi-monitor, `screen.width` puede ser el ancho del monitor principal, no del actual. Una alternativa es usar `window.screenX + (window.outerWidth - width) / 2` para centrar respecto a la ventana actual del browser.

> [!warning]
> No usar `window.open` sin `noopener` cuando se abren URLs externas. Sin `noopener`, la página externa tiene acceso a `window.opener` y puede redirigir la pestaña principal a una URL de phishing (tabnapping). Chrome 88+ aplica `noopener` automáticamente para `target="_blank"` en `<a>` sin `rel`, pero `window.open` aún requiere especificarlo manualmente.

## Notas relacionadas

- [[index|Window]] — visión general del objeto global
- [[03 alert, confirm, prompt|alert, confirm, prompt]] — otros diálogos y flujos modales
- [[../../06 Manejo de Eventos/index|Manejo de Eventos]] — event listeners y el evento `message`
