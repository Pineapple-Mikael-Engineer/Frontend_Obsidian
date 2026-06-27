---
title: Propiedades Específicas del Objeto Event — clientX, key y otras
aliases:
  - clientX
  - e.key
  - propiedades del evento
  - MouseEvent KeyboardEvent
tags:
  - javascript
  - api/concepto
  - eventos
draft: false
order: 4
---

# Propiedades Específicas (clientX, key)

> [!definicion]
> Cada subinterfaz de `Event` añade propiedades relevantes al tipo de interacción. `MouseEvent` expone coordenadas y botón; `KeyboardEvent` expone la tecla; `TouchEvent` expone puntos de contacto; `WheelEvent` expone el delta de desplazamiento; `InputEvent` expone el texto insertado. Las propiedades universales (`target`, `bubbles`, etc.) están disponibles en todas — se delegan a [[03 El Objeto Event/index|El Objeto Event]].

## MouseEvent

Eventos: `click`, `dblclick`, `mousedown`, `mouseup`, `mousemove`, `mouseover`, `mouseout`, `mouseenter`, `mouseleave`.

| Propiedad | Tipo | Descripción |
|---|---|---|
| `clientX` / `clientY` | number | Coordenadas relativas al viewport (px) |
| `pageX` / `pageY` | number | Coordenadas relativas al documento (incluye scroll) |
| `offsetX` / `offsetY` | number | Coordenadas relativas al borde del elemento target |
| `screenX` / `screenY` | number | Coordenadas relativas a la pantalla física |
| `button` | number | Botón del clic: 0 = izquierdo, 1 = rueda, 2 = derecho |
| `buttons` | number | Máscara de bits de botones actualmente presionados |
| `relatedTarget` | Element \| null | Elemento del que viene / al que va en `mouseover`/`mouseout` |
| `ctrlKey` / `shiftKey` / `altKey` / `metaKey` | boolean | Modificadores activos |

```js
canvas.addEventListener('mousemove', (e) => {
  const x = e.offsetX;  // relativo al canvas
  const y = e.offsetY;
  dibujar(x, y);
});
```

## KeyboardEvent

Eventos: `keydown`, `keyup`, `keypress` (obsoleto).

| Propiedad | Tipo | Descripción |
|---|---|---|
| `key` | string | Valor de la tecla legible: `'a'`, `'Enter'`, `'ArrowUp'`, `' '` |
| `code` | string | Tecla física en el teclado: `'KeyA'`, `'Enter'`, `'Space'` — independiente del idioma |
| `repeat` | boolean | `true` si la tecla se mantiene pulsada (autorepeat) |
| `keyCode` | number | Código numérico (obsoleto, usar `key` o `code`) |
| `ctrlKey` / `shiftKey` / `altKey` / `metaKey` | boolean | Modificadores activos |

```js
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') cerrarModal();
  if (e.key === 's' && (e.ctrlKey || e.metaKey)) {
    e.preventDefault();  // evitar guardar del navegador
    guardar();
  }
});
```

`key` devuelve el carácter producido según el layout del teclado activo. `code` devuelve la posición física de la tecla, sin importar el idioma. Para atajos de teclado, `key` es preferible; para juegos o cuando la posición importa, `code`.

## TouchEvent

Eventos: `touchstart`, `touchmove`, `touchend`, `touchcancel`.

| Propiedad | Tipo | Descripción |
|---|---|---|
| `touches` | TouchList | Todos los puntos de contacto activos en la pantalla |
| `targetTouches` | TouchList | Puntos de contacto sobre el elemento target |
| `changedTouches` | TouchList | Puntos que cambiaron en este evento |

Cada `Touch` en la lista expone `clientX`, `clientY`, `pageX`, `pageY`, `identifier` (id del dedo).

```js
superficie.addEventListener('touchmove', (e) => {
  e.preventDefault();  // evitar scroll
  const toque = e.changedTouches[0];
  moverObjeto(toque.clientX, toque.clientY);
}, { passive: false });
```

## WheelEvent

Evento: `wheel`.

| Propiedad | Tipo | Descripción |
|---|---|---|
| `deltaX` | number | Desplazamiento horizontal |
| `deltaY` | number | Desplazamiento vertical (positivo = abajo) |
| `deltaZ` | number | Desplazamiento en profundidad (raro) |
| `deltaMode` | number | Unidad: 0 = px, 1 = líneas, 2 = páginas |

```js
contenedor.addEventListener('wheel', (e) => {
  e.preventDefault();
  contenedor.scrollLeft += e.deltaY;  // scroll horizontal con la rueda
}, { passive: false });
```

## FocusEvent

Eventos: `focus`, `blur`, `focusin`, `focusout`.

| Propiedad | Tipo | Descripción |
|---|---|---|
| `relatedTarget` | Element \| null | Elemento que pierde el foco (`focus`) o que lo gana (`blur`) |

`focus` y `blur` no burbujean. `focusin` y `focusout` sí burbujean — usar éstos para delegación.

## InputEvent

Evento: `input`.

| Propiedad | Tipo | Descripción |
|---|---|---|
| `data` | string \| null | Texto insertado en esta operación (null en borrado) |
| `inputType` | string | Tipo de cambio: `'insertText'`, `'deleteContentBackward'`, `'insertFromPaste'`, etc. |

```js
editor.addEventListener('input', (e) => {
  if (e.inputType === 'insertText') {
    autocompletar(e.data);
  }
});
```

> [!tip]
> Para detectar modificadores en cualquier evento de ratón o teclado, `e.ctrlKey`, `e.shiftKey`, `e.altKey` y `e.metaKey` están disponibles también en `MouseEvent` y `WheelEvent`. `metaKey` es la tecla Cmd en macOS y la tecla Windows en PC.

> [!warning]
> `keyCode` y `charCode` están obsoletos y producen resultados inconsistentes entre navegadores y layouts de teclado. Usar siempre `e.key` (valor lógico) o `e.code` (posición física).

## Notas relacionadas

- [[03 El Objeto Event/index|El Objeto Event]]
- [[01 target vs currentTarget|target vs currentTarget]]
- [[01 Tipos de Eventos/index|Tipos de Eventos]]
