---
title: alert, confirm y prompt — diálogos bloqueantes
aliases:
  - window.alert
  - window.confirm
  - window.prompt
  - alert javascript
  - confirm javascript
  - prompt javascript
tags:
  - javascript
  - api/objeto
  - browser
draft: false
order: 3
---

# `alert`, `confirm` y `prompt`

> [!definicion]
> `alert`, `confirm` y `prompt` son los tres diálogos modales nativos del browser. Los tres **bloquean el hilo JS** — la ejecución del script se detiene hasta que el usuario cierra el diálogo. Su aspecto es controlado por el OS o el browser (no por CSS), y en producción se reemplazan casi siempre por modales custom o el elemento `<dialog>`.

```js
// alert — informar al usuario
window.alert("Operación completada.");
// Retorna undefined — no hay input del usuario

// confirm — pedir confirmación (OK / Cancelar)
const confirmado = window.confirm("¿Eliminar este elemento?");
if (confirmado) eliminar(); // true si OK, false si Cancelar o Escape

// prompt — pedir un valor de texto
const nombre = window.prompt("¿Cuál es tu nombre?", "Usuario");
// Retorna string (lo que escribió) o null (si canceló)
if (nombre !== null) {
  console.log(`Hola, ${nombre}`);
}
```

## Referencia de los tres métodos

| Método | Firma | Retorno | Descripción |
|---|---|---|---|
| `alert(mensaje)` | `(message?: string) => void` | `undefined` | Muestra el mensaje con un botón OK |
| `confirm(mensaje)` | `(message?: string) => boolean` | `true` / `false` | OK devuelve `true`; Cancelar o Escape devuelve `false` |
| `prompt(mensaje, default?)` | `(message: string, default?: string) => string \| null` | `string` o `null` | Muestra un campo de texto; Cancelar devuelve `null` |

El parámetro `message` acepta cualquier valor — se convierte a string con `toString()`. Si se pasa un objeto, se mostrará `[object Object]`, no el JSON.

## Por qué evitarlos en producción

Los tres comparten el mismo problema fundamental: **bloquean el hilo de renderizado**. Mientras el diálogo está abierto:

- El DOM no se actualiza.
- Las animaciones CSS se congelan.
- Los timers (`setTimeout`, `setInterval`) no se disparan.
- Las promesas pendientes no se resuelven.
- El Event Loop está parado.

Además del bloqueo, tienen otras limitaciones relevantes:

- No se pueden estilizar con CSS — el aspecto depende del OS y del browser.
- En iframes con políticas `sandbox` o cross-origin, los browsers los bloquean directamente.
- Safari puede suprimir `confirm` y `prompt` si se llaman repetidamente desde el mismo origen (protección anti-spam de diálogos).
- En algunos contextos (extensiones, Workers) no están disponibles.
- En `alert` y `confirm` el texto largo no tiene scroll en todos los browsers.

## Cuándo son aceptables

A pesar de todo, hay contextos donde siguen siendo útiles:

- **Scripts de consola del DevTools** — desarrollo y debugging rápido.
- **Bookmarklets** — scripts que no tienen UI propia.
- **Prototipos muy tempranos** — antes de montar la UI.
- **Scripts de automatización/Node con `prompt-sync`** — en CLI, no en browser.

En estos contextos, la simplicidad y el cero overhead de dependencias los hacen prácticos.

## Alternativa: el elemento `<dialog>` nativo

`<dialog>` es el reemplazo nativo moderno. Es accesible (trap de foco, Escape key, `aria-modal`), estilizable con CSS y no bloquea el hilo.

```html
<!-- En el HTML -->
<dialog id="miModal">
  <form method="dialog">
    <p>¿Confirmar la acción?</p>
    <button value="cancelar">Cancelar</button>
    <button value="confirmar">Confirmar</button>
  </form>
</dialog>
```

```js
const dialog = document.getElementById("miModal");

// Abrir como modal (bloquea interacción con el resto de la página vía ::backdrop)
dialog.showModal();

// Abrir como no-modal (el foco no queda atrapado)
dialog.show();

// Cerrar programáticamente y establecer un valor de retorno
dialog.close("confirmar");

// Leer el valor de retorno después de cerrar
dialog.addEventListener("close", () => {
  console.log(dialog.returnValue); // "confirmar" o "cancelar"
});
```

### Equivalentes modernos

| Diálogo clásico | Equivalente moderno |
|---|---|
| `alert(msg)` | `<dialog>` con un solo botón OK |
| `confirm(msg)` | `<dialog>` con `<form method="dialog">` y dos botones |
| `prompt(msg)` | `<dialog>` con `<input>` y `<form method="dialog">` |

## Cómo funciona por dentro

`alert`, `confirm` y `prompt` son operaciones del proceso del browser que suspenden el hilo JS mediante un mecanismo de IPC bloqueante entre el proceso renderer y el proceso browser (en Chromium). El renderer le dice al proceso browser "muestra este diálogo y espera respuesta", el proceso browser muestra la ventana del OS, y el renderer se queda bloqueado en espera. Cuando el usuario cierra el diálogo, el proceso browser devuelve la respuesta y el renderer reanuda la ejecución de JS.

Este modelo síncrono era natural cuando JS era single-threaded y no existían Promises. Hoy es un anti-patrón porque bloquea las microtasks, macrotasks y el compositor de frames, resultando en una página literalmente congelada (la pantalla no se actualiza, los videos se pausan, los cursores custom desaparecen).

## Receta: confirm reutilizable con `<dialog>`

```js
function confirmar(mensaje) {
  return new Promise((resolve) => {
    const dialog = document.createElement("dialog");
    dialog.innerHTML = `
      <p>${mensaje}</p>
      <form method="dialog">
        <button value="cancelar">Cancelar</button>
        <button value="confirmar" autofocus>Confirmar</button>
      </form>
    `;
    document.body.appendChild(dialog);
    dialog.showModal();

    dialog.addEventListener("close", () => {
      resolve(dialog.returnValue === "confirmar");
      dialog.remove();
    });
  });
}

// Uso — ahora es asíncrono, no bloquea
const aprobado = await confirmar("¿Eliminar el archivo?");
if (aprobado) eliminar();
```

> [!tip]
> `<dialog>` con `method="dialog"` en el form permite que los botones dentro del formulario cierren el diálogo automáticamente al hacer click, estableciendo `dialog.returnValue` al `value` del botón. Es la forma idiomática de implementar confirm/prompt nativos sin JS adicional para el cierre.

> [!warning]
> `window.prompt` devuelve `null` si el usuario cancela, pero devuelve `""` (string vacío) si acepta sin escribir nada. Hay que distinguir ambos casos: `resultado === null` es cancelar, `resultado === ""` es aceptar vacío. Un error común es comprobar solo `if (!resultado)` que trata ambos igual.

## Notas relacionadas

- [[index|Window]] — visión general del objeto global
- [[02 open y close|open y close]] — otras formas de abrir ventanas y modales externos
- [[../../06 Manejo de Eventos/index|Manejo de Eventos]] — eventos del DOM para interactividad sin bloqueo
