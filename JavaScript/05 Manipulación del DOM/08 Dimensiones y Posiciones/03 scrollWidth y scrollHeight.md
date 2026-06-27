---
title: scrollWidth y scrollHeight
aliases:
  - scrollWidth
  - scrollHeight
  - scrollTop
  - scrollLeft
tags:
  - javascript
  - api/propiedad
  - dom
draft: false
order: 3
---

# `scrollWidth` y `scrollHeight`

> [!definicion]
> `element.scrollWidth` y `element.scrollHeight` devuelven el ancho y el alto total del contenido desplazable del elemento, incluyendo la parte oculta por el overflow. Si el elemento no tiene scroll activo, `scrollWidth === clientWidth` y `scrollHeight === clientHeight`.

```js
const el = document.querySelector('.lista');

el.scrollWidth;    // ancho total del contenido (visible + desbordado)
el.scrollHeight;   // alto total del contenido (visible + desbordado)

el.scrollTop;      // px desplazados verticalmente (lectura/escritura)
el.scrollLeft;     // px desplazados horizontalmente (lectura/escritura)
```

## `scrollTop` y `scrollLeft`

Son de lectura **y escritura**. Asignarles un valor desplaza el contenido del elemento a esa posición:

```js
el.scrollTop = 0;       // ir al inicio vertical
el.scrollLeft = 0;      // ir al inicio horizontal

// Con animación suave
el.scrollTo({ top: 0, behavior: 'smooth' });
el.scrollTo({ top: el.scrollHeight, behavior: 'smooth' }); // ir al final
```

Los valores están acotados: `scrollTop` no puede ser negativo ni mayor que `scrollHeight - clientHeight`.

## Recetas comunes

### Detectar si el usuario llegó al final del contenedor

La condición exacta tiene un margen de `1px` por redondeo de subpíxel:

```js
el.addEventListener('scroll', () => {
  const alFinal = el.scrollTop + el.clientHeight >= el.scrollHeight - 1;
  if (alFinal) cargarMasContenido();
});
```

### Scroll infinito — cargar al acercarse al fondo

```js
el.addEventListener('scroll', () => {
  const distanciaAlFondo = el.scrollHeight - el.scrollTop - el.clientHeight;
  if (distanciaAlFondo < 200) cargarMasContenido(); // umbral de 200 px
});
```

### Saber si un elemento tiene contenido desbordado

```js
const desborda = el.scrollHeight > el.clientHeight;
```

### Medir el progreso de lectura de un artículo

```js
document.addEventListener('scroll', () => {
  const progreso = window.scrollY / (document.body.scrollHeight - window.innerHeight);
  barra.style.width = (progreso * 100).toFixed(1) + '%';
});
```

## Diferencia con `clientWidth` / `clientHeight`

`clientHeight` es el espacio visible; `scrollHeight` es el espacio total (incluyendo lo que hay fuera del viewport del elemento). Si `overflow: hidden`, el contenido puede estar desbordado y `scrollHeight > clientHeight`, pero el usuario no puede desplazarse.

```js
// Panel con overflow: auto y contenido largo
panel.clientHeight;  // 400 — altura visible
panel.scrollHeight;  // 1200 — altura total del contenido
panel.scrollTop;     // 0 — usuario en el inicio

// Sin desbordamiento
div.clientHeight === div.scrollHeight; // true cuando no hay overflow
```

## Cómo funciona por dentro

`scrollWidth` y `scrollHeight` se calculan incluyendo todo el contenido del subtree del elemento, aunque esté oculto por overflow. Se actualizan cuando el contenido cambia (DOM mutations, cambios de estilo). Como el resto de propiedades de layout, leerlas después de modificar el DOM fuerza un reflow síncrono.

> [!tip]
> Para hacer scroll a un elemento específico dentro de un contenedor, `elemento.scrollIntoView()` es más cómodo que calcular manualmente `scrollTop`. Ver [[05 Posiciones de Scroll]].

> [!warning]
> En elementos con `transform` o zoom CSS, `scrollHeight` puede no coincidir con lo esperado visualmente — se calcula en el espacio de coordenadas del layout, antes de la transformación.

## Notas relacionadas

- [[02 clientWidth y clientHeight]] — área visible sin contenido desbordado
- [[05 Posiciones de Scroll]] — `scrollY` del documento, `scrollIntoView`, `scrollTo`
- [[04 getBoundingClientRect]] — posición relativa al viewport
- [[08 Dimensiones y Posiciones/index|Dimensiones y Posiciones]]
- [[03 Reflows y Repaints]] — coste de leer propiedades de layout
