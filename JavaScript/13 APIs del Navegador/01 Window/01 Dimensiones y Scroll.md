---
title: window — Dimensiones y Scroll
aliases:
  - window dimensiones
  - window scroll
  - scrollTo
  - scrollBy
  - innerWidth
  - scrollY
tags:
  - javascript
  - api/objeto
  - browser
draft: false
---

# Dimensiones y Scroll en `window`

> [!definicion]
> `window` expone un conjunto de propiedades de solo lectura para conocer las dimensiones del viewport, de la ventana del browser y de la pantalla física, así como métodos para leer y controlar la posición del scroll. Estas propiedades son fundamentales para layouts dinámicos, detección de scroll y animaciones controladas por la posición de la ventana.

```js
// Dimensiones del viewport (área visible de la página)
window.innerWidth;   // p.ej. 1280
window.innerHeight;  // p.ej. 720

// Posición actual del scroll
window.scrollX; // píxeles desplazados horizontalmente
window.scrollY; // píxeles desplazados verticalmente

// Scroll a una posición absoluta
window.scrollTo({ top: 0, behavior: "smooth" });

// Scroll relativo
window.scrollBy({ top: 300, behavior: "smooth" });
```

## Propiedades de dimensiones

| Propiedad | Qué mide | Incluye scrollbar |
|---|---|---|
| `window.innerWidth` | Ancho del viewport (área de contenido visible) | Sí |
| `window.innerHeight` | Alto del viewport | Sí |
| `window.outerWidth` | Ancho total de la ventana del browser, incluyendo toolbars y bordes | — |
| `window.outerHeight` | Alto total de la ventana del browser | — |
| `window.screen.width` | Ancho de la pantalla física en píxeles CSS | — |
| `window.screen.height` | Alto de la pantalla física | — |
| `window.screen.availWidth` | Ancho de pantalla disponible (excluye taskbar del OS) | — |
| `window.screen.availHeight` | Alto de pantalla disponible | — |
| `window.devicePixelRatio` | Ratio píxeles físicos / píxeles CSS (2 en Retina, 3 en móviles premium) | — |

La diferencia entre `innerWidth` y `outerWidth` es el cromo del navegador: barras de herramientas, pestañas, scrollbar de la ventana del OS. En modo pantalla completa, `outerWidth === screen.width`.

`devicePixelRatio` es clave para canvas y gráficos: para que un canvas se vea nítido en Retina hay que multiplicar el ancho/alto del canvas por `devicePixelRatio` y escalar con CSS.

## Propiedades de scroll

| Propiedad / Método | Descripción |
|---|---|
| `window.scrollX` | Píxeles desplazados horizontalmente desde el inicio del documento (alias: `pageXOffset`) |
| `window.scrollY` | Píxeles desplazados verticalmente (alias: `pageYOffset`) |
| `window.scrollTo(x, y)` | Scroll a coordenadas absolutas del documento (forma abreviada) |
| `window.scrollTo({ top, left, behavior })` | Scroll a coordenadas absolutas con opciones |
| `window.scrollBy(x, y)` | Scroll relativo a la posición actual |
| `window.scrollBy({ top, left, behavior })` | Scroll relativo con opciones |
| `element.scrollIntoView(opciones)` | Desplaza la ventana para hacer visible el elemento |

El valor de `behavior` puede ser `"instant"` (sin animación, el default), `"smooth"` (animación CSS del browser) o `"auto"` (deja la decisión al browser según `scroll-behavior` del CSS).

`pageXOffset` y `pageYOffset` son aliases de `scrollX`/`scrollY` que existían antes. Son equivalentes pero `scrollX`/`scrollY` son los nombres modernos.

## Cómo funciona por dentro

El scroll de `window` mueve el viewport sobre el documento. `scrollY` refleja cuántos píxeles del documento han quedado "arriba" del borde visible. Internamente, el browser mantiene la posición del scroll como un desplazamiento del viewport sobre el layout del documento. Cuando se llama a `scrollTo` con `behavior: "smooth"`, el browser genera una animación usando su propio scheduler (fuera del Event Loop de JS) — el script continúa ejecutándose inmediatamente sin bloquear.

`element.scrollIntoView` delega en el browser el cálculo de la posición óptima. Con `block: "center"` el elemento queda centrado verticalmente en el viewport; con `block: "nearest"` el browser decide el mínimo scroll necesario para que el elemento sea visible.

Para obtener las dimensiones del documento completo (incluyendo parte invisible):

```js
const alturaDocumento = Math.max(
  document.body.scrollHeight, document.documentElement.scrollHeight,
  document.body.offsetHeight, document.documentElement.offsetHeight,
  document.body.clientHeight, document.documentElement.clientHeight
);
```

## Receta: detectar si el usuario llegó al fondo de la página

```js
function alFondo() {
  // scrollY: cuánto se ha scrolleado
  // innerHeight: alto del viewport
  // scrollHeight: alto total del documento
  return window.scrollY + window.innerHeight >= document.documentElement.scrollHeight - 10;
}

window.addEventListener("scroll", () => {
  if (alFondo()) {
    console.log("El usuario llegó al fondo — cargar más contenido");
    // Aquí: paginación infinita, lazy load, etc.
  }
});
```

El margen de 10px evita problemas de redondeo de subpíxeles en algunos browsers.

## Receta: botón "volver arriba" con smooth scroll

```js
const btnArriba = document.getElementById("btn-arriba");

// Mostrar el botón solo si el usuario ha bajado suficiente
window.addEventListener("scroll", () => {
  btnArriba.hidden = window.scrollY < 300;
});

// Al hacer clic, volver al tope
btnArriba.addEventListener("click", () => {
  window.scrollTo({ top: 0, behavior: "smooth" });
});
```

## Receta: canvas nítido en pantallas Retina

```js
const canvas = document.getElementById("mi-canvas");
const ctx = canvas.getContext("2d");
const dpr = window.devicePixelRatio ?? 1;

// Definir el tamaño lógico (CSS)
const anchoCSS = 400;
const altoCSS = 300;

// Escalar el buffer interno
canvas.width = anchoCSS * dpr;
canvas.height = altoCSS * dpr;
canvas.style.width = anchoCSS + "px";
canvas.style.height = altoCSS + "px";

// Escalar el contexto para que las coordenadas sigan siendo las lógicas
ctx.scale(dpr, dpr);
```

> [!tip]
> Para detectar cambios en el tamaño de la ventana, escuchar el evento `resize` en `window`. Usar `ResizeObserver` para elementos individuales — es más eficiente que escuchar `resize` global para cada componente. Dentro del listener de `resize`, hacer debounce para no ejecutar en cada frame: `window.addEventListener("resize", debounce(onResize, 150))`.

> [!warning]
> `scrollX` y `scrollY` son de solo lectura — asignarles un valor directamente no tiene efecto. Para cambiar el scroll hay que usar `scrollTo` o `scrollBy`. Además, animar el scroll manualmente con `setInterval`/`requestAnimationFrame` es innecesario: el `behavior: "smooth"` nativo es más eficiente porque corre en el compositor del browser sin bloquear el hilo JS.

## Notas relacionadas

- [[index|Window]] — visión general del objeto global
- [[../../05 Manipulación del DOM/08 Dimensiones y Posiciones/index|Dimensiones y Posiciones en el DOM]] — `getBoundingClientRect`, `offsetTop` y propiedades de elementos individuales
