---
title: getBoundingClientRect
aliases:
  - getBoundingClientRect
  - DOMRect
  - bounding rect
tags:
  - javascript
  - api/metodo
  - dom
draft: false
order: 4
---

# `getBoundingClientRect()`

> [!definicion]
> `element.getBoundingClientRect()` devuelve un objeto `DOMRect` con las propiedades `top`, `right`, `bottom`, `left`, `width`, `height`, `x` e `y`. Todas las coordenadas son relativas al **viewport** (esquina superior izquierda de la ventana visible), en punto flotante. Fuerza un reflow síncrono.

```js
const rect = el.getBoundingClientRect();

rect.top;     // distancia desde el borde superior del viewport al borde superior del elemento
rect.left;    // distancia desde el borde izquierdo del viewport al borde izquierdo del elemento
rect.right;   // rect.left + rect.width
rect.bottom;  // rect.top + rect.height
rect.width;   // ancho del border box (igual que offsetWidth pero en punto flotante)
rect.height;  // alto del border box
rect.x;       // alias de rect.left
rect.y;       // alias de rect.top
```

## Coordenadas relativas al viewport vs al documento

Las coordenadas de `getBoundingClientRect()` cambian cuando el usuario hace scroll: `top` decrece al desplazarse hacia abajo. Para obtener la posición **relativa al documento** (fija respecto al scroll) se suma el desplazamiento actual:

```js
const rect = el.getBoundingClientRect();

const posDoc = {
  top:  rect.top  + window.scrollY,
  left: rect.left + window.scrollX,
};
```

## Recetas comunes

### Posicionar un tooltip relativo a un elemento

```js
function posicionarTooltip(ancla, tooltip) {
  const rect = ancla.getBoundingClientRect();
  tooltip.style.top  = rect.bottom + window.scrollY + 8 + 'px';
  tooltip.style.left = rect.left   + window.scrollX + 'px';
}
```

### Detectar si un elemento es visible en el viewport

```js
function esVisible(el) {
  const r = el.getBoundingClientRect();
  return r.top < window.innerHeight && r.bottom > 0
      && r.left < window.innerWidth  && r.right  > 0;
}
```

La función devuelve `true` si al menos un píxel del elemento es visible. Para detectar visibilidad completa: `r.top >= 0 && r.bottom <= window.innerHeight`.

### Calcular la posición relativa entre dos elementos

```js
function posicionRelativa(hijo, padre) {
  const rHijo  = hijo.getBoundingClientRect();
  const rPadre = padre.getBoundingClientRect();
  return {
    top:  rHijo.top  - rPadre.top,
    left: rHijo.left - rPadre.left,
  };
}
```

## `DOMRect` es inmutable

El objeto devuelto es una instantánea. Sus propiedades no se pueden modificar directamente, y no se actualiza si el elemento se mueve después de la llamada. Cada llamada a `getBoundingClientRect()` genera una nueva lectura del layout.

```js
const rect = el.getBoundingClientRect();
rect.top = 0; // sin efecto — DOMRect es de solo lectura
```

## Coste y layout thrashing

`getBoundingClientRect()` fuerza al motor a finalizar cualquier cambio de layout pendiente antes de devolver el valor (reflow síncrono). Leerlo dentro de un bucle de escrituras es especialmente dañino:

```js
// MAL — reflow en cada iteración
elements.forEach(el => {
  const rect = el.getBoundingClientRect(); // fuerza reflow
  el.style.height = rect.width + 'px';    // escritura
});

// BIEN — todas las lecturas juntas, luego escrituras
const rects = elements.map(el => el.getBoundingClientRect());
elements.forEach((el, i) => { el.style.height = rects[i].width + 'px'; });
```

Para visibilidad continua de múltiples elementos, `IntersectionObserver` es más eficiente porque el navegador lo procesa de forma asíncrona, sin reflows síncronos.

## Cómo funciona por dentro

El motor retiene un árbol de layout calculado. Cuando se modifican estilos o el DOM, ese árbol se marca como "sucio". Llamar a `getBoundingClientRect()` obliga a recalcular el layout antes de leer, aunque el navegador hubiera podido diferir ese cálculo hasta el próximo frame. Por eso se dice que "fuerza un reflow".

> [!tip]
> Para detectar si un elemento entra o sale del viewport de forma continua y eficiente, usar `IntersectionObserver` en lugar de `getBoundingClientRect()` en el handler de scroll.

> [!warning]
> En elementos con `display: none`, `getBoundingClientRect()` devuelve un `DOMRect` con todos los valores a `0`. Verificar la visibilidad antes de confiar en el resultado.

## Notas relacionadas

- [[01 offsetWidth y offsetHeight]] — tamaño en enteros, relativo al offsetParent
- [[05 Posiciones de Scroll]] — `window.scrollY` / `scrollX` para convertir a coordenadas de documento
- [[03 Reflows y Repaints]] — por qué fuerza reflow y cómo evitar thrashing
- [[08 Dimensiones y Posiciones/index|Dimensiones y Posiciones]]
