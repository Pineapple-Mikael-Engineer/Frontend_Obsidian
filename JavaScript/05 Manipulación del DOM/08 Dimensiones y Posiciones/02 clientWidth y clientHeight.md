---
title: clientWidth y clientHeight
aliases:
  - clientWidth
  - clientHeight
  - clientTop
  - clientLeft
tags:
  - javascript
  - api/propiedad
  - dom
draft: false
order: 2
---

# `clientWidth` y `clientHeight`

> [!definicion]
> `element.clientWidth` y `element.clientHeight` devuelven el ancho y el alto del área de contenido **visible** del elemento: incluyen padding pero excluyen border, margin y la scrollbar. Son de solo lectura y se expresan en píxeles enteros.

```js
const el = document.querySelector('.panel');

el.clientWidth;   // ancho de contenido + padding, sin border ni scrollbar
el.clientHeight;  // alto de contenido + padding, sin border ni scrollbar

el.clientTop;     // grosor del border-top (en px)
el.clientLeft;    // grosor del border-left (en px)
```

## Tabla comparativa: offset vs client vs scroll

| Propiedad | Border | Padding | Scrollbar | Contenido oculto |
|---|---|---|---|---|
| `offsetWidth` | Sí | Sí | Sí | No |
| `clientWidth` | No | Sí | No | No |
| `scrollWidth` | No | Sí | No | Sí |

## `clientTop` y `clientLeft`

`clientTop` equivale al valor computado de `border-top-width`; `clientLeft`, a `border-left-width`. En la práctica se usan poco — es más directo leer el estilo computado con `getComputedStyle`. Su utilidad aparece cuando se necesita la distancia desde el borde exterior al área de contenido sin pasar por CSS.

```js
// Para un elemento con border: 4px solid
el.clientTop;   // 4
el.clientLeft;  // 4
```

## Tamaño del viewport

Para el elemento raíz (`document.documentElement`), `clientWidth` y `clientHeight` devuelven el tamaño del viewport **sin** incluir la scrollbar. Es la forma estándar y fiable de obtener las dimensiones de la ventana visible:

```js
const vpAncho = document.documentElement.clientWidth;
const vpAlto  = document.documentElement.clientHeight;
```

`window.innerWidth` y `window.innerHeight` son similares pero **incluyen** el ancho de la scrollbar. En diseño responsivo donde importa el espacio real disponible para contenido, `document.documentElement.clientWidth` es la referencia correcta.

```js
// Diferencia cuando hay scrollbar vertical visible (~15-17 px)
window.innerWidth;                              // incluye scrollbar
document.documentElement.clientWidth;           // excluye scrollbar
```

## Receta: detectar si un elemento tiene scrollbar

Si `el.scrollHeight > el.clientHeight`, el elemento tiene contenido desbordado y (si `overflow` no es `hidden`) una scrollbar activa.

```js
function tieneScrollVertical(el) {
  return el.scrollHeight > el.clientHeight;
}
```

## Cómo funciona por dentro

Como `offsetWidth`, `clientWidth` se calcula durante el layout pass. La exclusión de la scrollbar es intencional: representa el espacio real disponible para el contenido. En sistemas donde la scrollbar es overlay (macOS con trackpad, móviles), `clientWidth` y `offsetWidth` pueden coincidir.

> [!tip]
> Para obtener las dimensiones del viewport de forma consistente entre navegadores y evitar el ancho de la scrollbar, usar `document.documentElement.clientWidth` / `clientHeight` en lugar de `window.innerWidth` / `innerHeight`.

> [!warning]
> Para elementos inline (`display: inline`), `clientWidth` y `clientHeight` son `0`. Estas propiedades solo tienen sentido para elementos de bloque o inline-block.

## Notas relacionadas

- [[01 offsetWidth y offsetHeight]] — incluye border; relativo al offsetParent
- [[03 scrollWidth y scrollHeight]] — incluye contenido desbordado
- [[04 getBoundingClientRect]] — posición y tamaño en punto flotante
- [[08 Dimensiones y Posiciones/index|Dimensiones y Posiciones]]
