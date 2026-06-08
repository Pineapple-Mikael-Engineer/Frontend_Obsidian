---
title: Eventos de Teclado
aliases:
  - KeyboardEvent
  - eventos del teclado
tags:
  - javascript
  - api/evento
  - eventos
draft: false
---

# Eventos de Teclado

> [!definicion]
> Los eventos de teclado son instancias de `KeyboardEvent` que el navegador dispara cuando el usuario presiona o suelta una tecla. Las propiedades `key` y `code` son los campos modernos: `key` da el valor lógico producido (depende del layout del teclado), mientras que `code` identifica la tecla física independientemente del idioma o disposición del teclado.

## Tabla de eventos

| Evento | Cuándo se dispara | Burbujea | Estado |
|---|---|---|---|
| keydown | Tecla presionada (antes de que el carácter aparezca en el input) | Sí | Vigente |
| keyup | Tecla soltada | Sí | Vigente |
| keypress | Tecla presionada que produce un carácter (no dispara en Shift, Ctrl, etc.) | Sí | Obsoleto |

El orden de disparo es `keydown → keypress (si aplica) → [acción del navegador] → keyup`.

## Propiedades clave del KeyboardEvent

```javascript
document.addEventListener('keydown', (e) => {
  console.log('key:     ', e.key);      // "a", "A", "Enter", "ArrowUp", " "
  console.log('code:    ', e.code);     // "KeyA", "Enter", "ArrowUp", "Space"
  console.log('keyCode: ', e.keyCode);  // 65, 13, 38, 32 — OBSOLETO
  console.log('repeat:  ', e.repeat);  // true si la tecla está mantenida
  console.log('ctrl:    ', e.ctrlKey);
  console.log('shift:   ', e.shiftKey);
  console.log('alt:     ', e.altKey);
  console.log('meta:    ', e.metaKey); // tecla Cmd en macOS, tecla Win en Windows
});
```

### key — valor lógico del carácter producido

`key` devuelve el valor de la tecla según el layout activo del teclado y el estado de Shift/CapsLock. Algunos valores especiales:

| Tecla física | key (layout ES) | key (layout EN) | key con Shift (ES) |
|---|---|---|---|
| KeyZ | "y" | "z" | "Y" |
| BracketLeft | "'" | "[" | "?" |
| Enter | "Enter" | "Enter" | "Enter" |
| Space | " " | " " | " " |
| ArrowUp | "ArrowUp" | "ArrowUp" | "ArrowUp" |
| Escape | "Escape" | "Escape" | "Escape" |

Para teclas especiales, `key` usa nombres estandarizados: `"Enter"`, `"Escape"`, `"Tab"`, `"Backspace"`, `"Delete"`, `"ArrowUp"`, `"ArrowDown"`, `"ArrowLeft"`, `"ArrowRight"`, `"Home"`, `"End"`, `"PageUp"`, `"PageDown"`, `"F1"`…`"F12"`, `"Shift"`, `"Control"`, `"Alt"`, `"Meta"`.

### code — identificador de la tecla física

`code` identifica la posición física de la tecla en un teclado QWERTY de referencia, sin importar el layout activo. Útil para atajos de juegos donde importa la posición de la tecla (WASD), no el carácter que produce.

```javascript
// En un teclado AZERTY francés, la tecla física donde está "Q" en QWERTY
// produce e.key === "a" pero e.code === "KeyQ"
document.addEventListener('keydown', (e) => {
  if (e.code === 'KeyW') moverArriba();   // siempre la tecla W física
  if (e.code === 'KeyA') moverIzquierda();
  if (e.code === 'KeyS') moverAbajo();
  if (e.code === 'KeyD') moverDerecha();
});
```

### repeat — tecla mantenida

`e.repeat` es `true` cuando el evento `keydown` se está disparando de forma repetida porque el usuario mantiene la tecla pulsada. El navegador genera múltiples `keydown` con `repeat: true` pero solo un `keyup` al soltar.

```javascript
document.addEventListener('keydown', (e) => {
  if (e.repeat) return; // ignorar repeticiones de auto-repeat
  // lógica que solo debe ejecutarse una vez al presionar
});
```

## key vs code: cuándo usar cada uno

- Usar `e.key` para leer texto ingresado por el usuario, validar caracteres, detectar teclas especiales como `Enter`, `Escape`, `Tab`. El valor es el que el usuario ve producido.
- Usar `e.code` para atajos de teclado posicionales (juegos, editors) donde la posición física importa más que el carácter. Un atajo `Ctrl+Z` basado en `e.code === "KeyZ"` funcionará igual en teclados español, alemán y japonés aunque `e.key` sea diferente.
- Para atajos de teclado estándar de aplicación (Ctrl+S, Ctrl+Z, Ctrl+C), usar `e.key` con `e.ctrlKey` es más robusto porque respeta el mapping del sistema operativo.

## keypress está obsoleto

`keypress` no se dispara para teclas que no producen caracteres (flechas, Escape, Ctrl, etc.), tiene comportamiento inconsistente entre navegadores y fue removido de la especificación. Toda su funcionalidad está cubierta por `keydown` y `keyup`.

## Receta: atajos de teclado

```javascript
document.addEventListener('keydown', (e) => {
  const ctrl = e.ctrlKey || e.metaKey; // Ctrl en Win/Linux, Cmd en Mac

  // Guardar: Ctrl/Cmd + S
  if (ctrl && e.key === 's') {
    e.preventDefault(); // evita el diálogo de guardado del navegador
    guardarDocumento();
    return;
  }

  // Cerrar modal: Escape
  if (e.key === 'Escape') {
    cerrarModal();
    return;
  }

  // Deshacer: Ctrl/Cmd + Z
  if (ctrl && e.key === 'z') {
    e.preventDefault();
    deshacer();
    return;
  }
});
```

### Navegar una lista con flechas

```javascript
const items = document.querySelectorAll('.lista-item');
let indiceActivo = 0;

document.addEventListener('keydown', (e) => {
  if (e.key === 'ArrowDown') {
    e.preventDefault(); // evita scroll de la página
    indiceActivo = Math.min(indiceActivo + 1, items.length - 1);
    actualizarFoco(indiceActivo);
  }
  if (e.key === 'ArrowUp') {
    e.preventDefault();
    indiceActivo = Math.max(indiceActivo - 1, 0);
    actualizarFoco(indiceActivo);
  }
  if (e.key === 'Enter') {
    items[indiceActivo].click();
  }
});

function actualizarFoco(idx) {
  items.forEach((item, i) => item.classList.toggle('activo', i === idx));
  items[idx].scrollIntoView({ block: 'nearest' });
}
```

## Receta: prevenir envío con Enter en textarea

`<textarea>` no envía el formulario con Enter (inserta un salto de línea), pero `<input type="text">` sí. Para prevenir el envío accidental en un input de una sola línea dentro de un form complejo:

```javascript
const inputBusqueda = document.getElementById('busqueda');

inputBusqueda.addEventListener('keydown', (e) => {
  if (e.key === 'Enter') {
    e.preventDefault(); // no envía el form
    realizarBusqueda(e.target.value);
  }
});
```

Para el caso inverso (permitir nuevas líneas en un textarea pero enviar con Ctrl+Enter):

```javascript
const textarea = document.getElementById('comentario');

textarea.addEventListener('keydown', (e) => {
  if ((e.ctrlKey || e.metaKey) && e.key === 'Enter') {
    e.preventDefault();
    document.getElementById('form-comentario').dispatchEvent(new Event('submit'));
  }
});
```

## Cómo funciona por dentro

Cuando el usuario presiona una tecla, el sistema operativo envía un key scan code al navegador. El navegador lo mapea a un código de tecla física (`code`) y luego consulta el layout activo del teclado para producir el valor lógico (`key`). El evento `keydown` se despacha antes de que el navegador procese la acción asociada (como insertar el carácter en un input), por eso `e.preventDefault()` puede cancelar esa acción. El evento `keyup` se despacha después de que el sistema libera la tecla, sin capacidad de cancelar nada útil.

> [!tip]
> Para detectar combinaciones multi-tecla como Ctrl+Shift+K, combinar las propiedades `ctrlKey`, `shiftKey` y `e.key` en una sola condición: `e.ctrlKey && e.shiftKey && e.key === 'K'`. El orden de `e.key` será la tecla no modificadora con shift aplicado (`"K"` mayúscula en este caso).

> [!warning]
> `e.keyCode` y `e.which` están obsoletos y no deben usarse en código nuevo. Los valores difieren entre navegadores para teclas no ASCII y no están documentados de forma consistente. Migrar todo código legacy a `e.key` y `e.code`.

## Notas relacionadas

- [[index]]
- [[03 Eventos de Formulario]]
- [[01 Eventos de Ratón]]
