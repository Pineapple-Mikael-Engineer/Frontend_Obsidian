---
title: Posiciones de Scroll
aliases:
  - scrollY
  - scrollX
  - pageYOffset
  - scrollTo
  - scrollBy
  - scrollIntoView
tags:
  - javascript
  - api/propiedad
  - dom
draft: false
---

# Posiciones de Scroll

> [!definicion]
> El navegador expone propiedades y métodos en `window` y en cada `Element` para leer y controlar el desplazamiento del documento y de los contenedores con overflow. `window.scrollY` / `window.scrollX` representan los píxeles desplazados del documento; sus contrapartes en un elemento son `el.scrollTop` / `el.scrollLeft`.

```js
// Leer posición del documento
window.scrollY;   // px desde el tope del documento (equivale a pageYOffset)
window.scrollX;   // px desde el borde izquierdo del documento

// Desplazar el documento
window.scrollTo(0, 500);                             // x=0, y=500 px
window.scrollTo({ top: 500, left: 0, behavior: 'smooth' });
window.scrollBy(0, 200);                             // relativo a posición actual
window.scrollBy({ top: 200, behavior: 'smooth' });

// Desplazar un elemento
el.scrollTop  = 0;                                   // al inicio vertical
el.scrollTo({ top: el.scrollHeight, behavior: 'smooth' }); // al final
```

## `window.scrollY` y `window.scrollX`

Son los alias modernos de `window.pageYOffset` / `window.pageXOffset`. `pageYOffset` sigue funcionando pero `scrollY` es la forma recomendada. No confundir con `document.documentElement.scrollTop`: en la práctica equivalen, pero `scrollY` es siempre fiable en todos los navegadores.

```js
// Obtener posición absoluta de un elemento en el documento
const rect = el.getBoundingClientRect();
const topEnDocumento  = rect.top  + window.scrollY;
const leftEnDocumento = rect.left + window.scrollX;
```

## `window.scrollTo()` y `window.scrollBy()`

`scrollTo` hace scroll a una posición **absoluta**; `scrollBy` lo hace **relativo** a la posición actual. Ambos aceptan la forma de dos argumentos `(x, y)` o un objeto de opciones con `behavior`:

| `behavior` | Efecto |
|---|---|
| `'auto'` | Salto instantáneo (por defecto) |
| `'smooth'` | Animación suave gestionada por el navegador |

```js
// Volver al inicio de la página con animación
window.scrollTo({ top: 0, behavior: 'smooth' });

// Avanzar una pantalla hacia abajo
window.scrollBy({ top: window.innerHeight, behavior: 'smooth' });
```

## `element.scrollIntoView()`

Hace scroll hasta que el elemento sea visible en el viewport. Acepta un objeto de opciones:

```js
el.scrollIntoView();
el.scrollIntoView(true);                                    // alinear al tope
el.scrollIntoView({ behavior: 'smooth', block: 'center' }); // centrado vertical
el.scrollIntoView({ behavior: 'smooth', block: 'nearest' }); // mínimo desplazamiento
```

| Opción | Valores | Efecto |
|---|---|---|
| `behavior` | `'auto'`, `'smooth'` | Animación o salto |
| `block` | `'start'`, `'center'`, `'end'`, `'nearest'` | Alineación vertical |
| `inline` | `'start'`, `'center'`, `'end'`, `'nearest'` | Alineación horizontal |

`block: 'nearest'` es el valor más eficiente: solo desplaza si el elemento no es ya visible, y el mínimo necesario.

## Scroll de un elemento (no del documento)

Las propiedades `scrollTop` y `scrollLeft` de un elemento funcionan igual que las del documento pero dentro del contenedor. El método `scrollTo()` también está disponible en elementos:

```js
const panel = document.querySelector('.panel');

panel.scrollTop;                                      // px desplazados verticalmente
panel.scrollTop = 0;                                  // al inicio
panel.scrollTo({ top: panel.scrollHeight, behavior: 'smooth' }); // al final
```

Ver [[03 scrollWidth y scrollHeight]] para la detección del final de scroll de un elemento.

## Recetas comunes

### Ancla con scroll suave

```js
document.querySelectorAll('a[href^="#"]').forEach(a => {
  a.addEventListener('click', e => {
    e.preventDefault();
    const destino = document.querySelector(a.getAttribute('href'));
    destino?.scrollIntoView({ behavior: 'smooth', block: 'start' });
  });
});
```

### Botón "volver al inicio"

```js
btnTop.addEventListener('click', () => {
  window.scrollTo({ top: 0, behavior: 'smooth' });
});
```

### Mostrar botón solo al bajar cierta distancia

```js
window.addEventListener('scroll', () => {
  btnTop.hidden = window.scrollY < 400;
}, { passive: true });
```

El evento de scroll en `window` es frecuente — marcar el listener como `{ passive: true }` para que el navegador sepa que no se llamará `preventDefault()` y pueda optimizar el scroll.

## Cómo funciona por dentro

`window.scrollY` se lee del compositor del navegador y no fuerza un reflow: es una propiedad de solo lectura que el motor actualiza después de cada frame de scroll. En contraste, `element.scrollTop` leído justo después de modificar el DOM puede forzar un layout pass.

> [!tip]
> Usar `{ passive: true }` en todos los listeners de scroll para no bloquear el hilo de composición. El navegador puede realizar el scroll en un hilo separado; un listener no-pasivo obliga a esperar la ejecución del JS antes de desplazar la página.

> [!warning]
> `scroll-behavior: smooth` en CSS aplica `smooth` globalmente a todos los scrolls del documento o elemento, incluyendo clics en anclas. Si se necesita control granular por acción, es mejor no establecerlo en CSS y gestionarlo por JS con la opción `behavior`.

## Notas relacionadas

- [[03 scrollWidth y scrollHeight]] — detectar fin de scroll, scroll infinito
- [[04 getBoundingClientRect]] — convertir coordenadas de viewport a documento con `scrollY`
- [[08 Dimensiones y Posiciones/index|Dimensiones y Posiciones]]
