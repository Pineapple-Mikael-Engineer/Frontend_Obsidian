---
title: offsetWidth y offsetHeight
aliases:
  - offsetWidth
  - offsetHeight
  - offsetTop
  - offsetLeft
  - offsetParent
tags:
  - javascript
  - api/propiedad
  - dom
draft: false
---

# `offsetWidth` y `offsetHeight`

> [!definicion]
> `element.offsetWidth` y `element.offsetHeight` devuelven el ancho y el alto del elemento en píxeles enteros, incluyendo padding y border pero sin margin. Son de solo lectura. Se calculan durante el layout pass del motor y representan el tamaño del **border box** del elemento.

```js
const el = document.querySelector('.card');

el.offsetWidth;   // ej. 320  — ancho con padding + border
el.offsetHeight;  // ej. 200  — alto con padding + border

el.offsetTop;     // distancia al borde superior del offsetParent
el.offsetLeft;    // distancia al borde izquierdo del offsetParent
el.offsetParent;  // referencia al ancestro posicionado más cercano
```

## Propiedades del grupo `offset*`

| Propiedad | Qué devuelve | Tipo |
|---|---|---|
| `offsetWidth` | Ancho del border box (padding + border incluidos) | Entero (px) |
| `offsetHeight` | Alto del border box (padding + border incluidos) | Entero (px) |
| `offsetTop` | Distancia superior al `offsetParent` | Entero (px) |
| `offsetLeft` | Distancia izquierda al `offsetParent` | Entero (px) |
| `offsetParent` | Ancestro posicionado más cercano (o `<body>`) | `Element` o `null` |

## `offsetParent` y referencial de posición

`offsetTop` y `offsetLeft` son relativos al `offsetParent`, no al viewport ni al documento. El `offsetParent` es el ancestro más cercano con `position` distinto de `static` (`relative`, `absolute`, `fixed`, `sticky`). Si ningún ancestro está posicionado, el `offsetParent` es `<body>`.

```js
// Un elemento hijo absoluto dentro de un contenedor relative
const contenedor = document.querySelector('.contenedor'); // position: relative
const hijo = contenedor.querySelector('.hijo');           // position: absolute

hijo.offsetParent === contenedor; // true
hijo.offsetTop;                   // distancia al borde superior de .contenedor
```

> [!warning]
> `offsetTop` no es la posición desde el borde de la ventana ni desde el tope del documento. En árboles anidados con múltiples ancestros posicionados, el valor puede confundir. Para obtener la posición real respecto al viewport usar [[04 getBoundingClientRect]].

## Obtener posición relativa al documento

Para calcular la posición de un elemento respecto al documento completo (no al `offsetParent`) se suman `offsetTop` y `offsetLeft` de cada ancestro hasta llegar a `body`. En la práctica es más robusto y legible combinar `getBoundingClientRect()` con el scroll actual:

```js
function posicionEnDocumento(el) {
  const rect = el.getBoundingClientRect();
  return {
    top:  rect.top  + window.scrollY,
    left: rect.left + window.scrollX,
  };
}
```

## Cómo funciona por dentro

El motor calcula estas propiedades durante el **layout pass** (también llamado reflow). El resultado es siempre un entero — los valores subpíxel se redondean, a diferencia de `getBoundingClientRect()` que devuelve punto flotante. Leer `offsetWidth` u `offsetHeight` después de haber modificado el DOM o los estilos fuerza un reflow síncrono inmediato.

```js
// MAL — fuerza reflow en cada iteración
items.forEach(item => {
  item.style.width = item.offsetWidth + 10 + 'px';
});

// BIEN — todas las lecturas primero
const anchos = items.map(item => item.offsetWidth);
items.forEach((item, i) => { item.style.width = anchos[i] + 10 + 'px'; });
```

> [!tip]
> Para elementos con `display: none`, `offsetWidth` y `offsetHeight` devuelven `0`. Si el elemento está en el DOM pero oculto con `visibility: hidden`, las dimensiones sí se calculan normalmente.

> [!warning]
> `offsetParent` devuelve `null` para el elemento `<body>`, para elementos con `display: none`, y para elementos con `position: fixed` (cuyo offsetParent es `null` en la mayoría de navegadores).

## Notas relacionadas

- [[04 getBoundingClientRect]] — coordenadas relativas al viewport en punto flotante
- [[02 clientWidth y clientHeight]] — área visible sin border
- [[08 Dimensiones y Posiciones/index|Dimensiones y Posiciones]]
- [[03 Reflows y Repaints]] — coste de leer propiedades de layout
