---
title: Reflows y Repaints
aliases:
  - reflow
  - repaint
  - layout thrashing
  - composite
tags:
  - javascript
  - api/concepto
  - dom
draft: false
---

# Reflows y Repaints

> [!definicion]
> **Reflow** (o layout): el motor recalcula la geometría del documento — posiciones, tamaños, relaciones entre nodos. Es la operación más costosa porque puede afectar a todo el árbol. **Repaint**: el motor redibuja los píxeles sin recalcular la geometría — ocurre cuando cambia color, fondo o visibilidad. **Composite**: el motor recompone capas gráficas sin reflow ni repaint — es la operación más barata.

## El pipeline de renderizado

```
JavaScript → Style → Layout → Paint → Composite
```

Cada etapa puede desencadenar las siguientes. Los cambios que afectan solo al composite (como `transform` y `opacity`) omiten Layout y Paint — son los más eficientes para animar.

## Qué dispara cada etapa

| Operación | Layout | Paint | Composite |
|---|---|---|---|
| Cambiar `width`, `height`, `margin`, `padding` | Sí | Sí | Sí |
| Cambiar `color`, `background-color`, `border-color` | No | Sí | Sí |
| Cambiar `transform`, `opacity` | No | No | Sí |
| Leer `offsetWidth`, `clientHeight`, `getBoundingClientRect` después de escribir | Reflow forzado | — | — |
| Añadir / eliminar nodos del DOM | Sí | Sí | Sí |
| Cambiar `font-size`, `font-family` | Sí | Sí | Sí |
| Cambiar `visibility` | No | Sí | Sí |
| Cambiar `display: none` ↔ block | Sí | Sí | Sí |

## Layout thrashing

Se produce cuando se leen propiedades geométricas inmediatamente después de escribir estilos o modificar el DOM, obligando al motor a finalizar el reflow diferido antes de devolver el valor.

```js
// MAL — layout thrashing: reflow en cada iteración
elementos.forEach(el => {
  el.style.width = el.offsetWidth + 10 + 'px'; // lectura fuerza reflow; escritura lo invalida
});

// BIEN — todas las lecturas primero, luego todas las escrituras
const anchos = elementos.map(el => el.offsetWidth);         // fase de lectura
elementos.forEach((el, i) => {
  el.style.width = anchos[i] + 10 + 'px';                  // fase de escritura
});
```

## Propiedades que fuerzan reflow al leerlas

| Propiedad / Método | Tipo |
|---|---|
| `offsetWidth`, `offsetHeight`, `offsetTop`, `offsetLeft` | Propiedad |
| `clientWidth`, `clientHeight`, `clientTop`, `clientLeft` | Propiedad |
| `scrollWidth`, `scrollHeight`, `scrollTop`, `scrollLeft` | Propiedad |
| `getBoundingClientRect()` | Método |
| `getComputedStyle()` | Función |
| `scrollIntoView()` | Método |
| `focus()` | Método |

Estas propiedades siempre devuelven el valor actual — si el layout está marcado como "sucio" por una escritura previa, el motor lo recalcula antes de responder.

## Propiedades CSS y su coste

| CSS | Etapa | Coste |
|---|---|---|
| `width`, `height`, `top`, `left`, `margin`, `padding`, `border-width`, `font-size` | Layout → Paint → Composite | Alto |
| `color`, `background`, `border-color`, `outline`, `box-shadow`, `visibility` | Paint → Composite | Medio |
| `transform`, `opacity`, `filter` (algunos) | Solo Composite | Bajo |

## `will-change` y capas GPU

`will-change: transform` es un hint al navegador para promover el elemento a su propia capa de composición. Las animaciones en capas propias se ejecutan en el hilo del compositor sin afectar al layout principal:

```css
.animado {
  will-change: transform; /* promover a capa GPU */
}
```

```js
// Activar dinámicamente antes de la animación y desactivar al terminar
el.style.willChange = 'transform';
// ... animar ...
el.style.willChange = 'auto'; // liberar la capa al terminar
```

> [!warning]
> No aplicar `will-change` a todos los elementos ni dejarlo activo permanentemente. Cada capa GPU consume memoria de vídeo. Reservarlo para elementos que realmente se animan con frecuencia.

## Receta: separar lecturas y escrituras con rAF

Para operaciones complejas que mezclan lecturas y escrituras en distintos frames:

```js
// Leer en el frame actual
const alturas = elementos.map(el => el.offsetHeight);

// Escribir en el siguiente frame (el layout ya está estabilizado)
requestAnimationFrame(() => {
  elementos.forEach((el, i) => {
    el.style.height = alturas[i] * 1.5 + 'px';
  });
});
```

## Herramientas de diagnóstico

El panel **Performance** de DevTools permite grabar una sesión y ver cada reflow y repaint en la línea de tiempo. Los reflows aparecen como bloques violeta ("Layout"); los repaints, como bloques verdes ("Paint"). Un reflow forzado síncronamente aparece marcado en rojo con la etiqueta "Forced reflow".

Pasos básicos:
1. Abrir DevTools → pestaña Performance.
2. Hacer clic en "Record" y reproducir la interacción.
3. Buscar bloques violeta frecuentes o encadenados — indican layout thrashing.

> [!tip]
> Las animaciones de `transform` y `opacity` son las únicas garantizadas para ejecutarse solo en el compositor en todos los navegadores modernos. Para cualquier otra propiedad CSS, comprobar en csstriggers.com qué etapas desencadena.

> [!warning]
> Incluso `opacity` puede desencadenar un repaint si el elemento no tiene su propia capa. Aplicar `will-change: opacity` o `transform: translateZ(0)` para forzar la promoción a capa propia antes de animar.

## Notas relacionadas

- [[01 DocumentFragment]] — reducir reflows al insertar muchos nodos
- [[02 requestAnimationFrame]] — sincronizar escrituras con el ciclo de renderizado
- [[04 getBoundingClientRect]] — ejemplo clásico de propiedad que fuerza reflow
- [[01 offsetWidth y offsetHeight]] — propiedades de layout que fuerzan reflow
- [[09 Actualizaciones Eficientes/index|Actualizaciones Eficientes]]
