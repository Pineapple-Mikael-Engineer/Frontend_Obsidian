---
title: Observers — APIs reactivas al DOM y al viewport
aliases:
  - Observers
  - Observer APIs
tags:
  - javascript
  - api/concepto
  - browser
draft: false
---

# Observers

> [!definicion]
> Los **Observers** son APIs del navegador que permiten reaccionar a cambios en el DOM o en el tamaño/visibilidad de elementos de forma asíncrona y eficiente, sin necesidad de polling con `setInterval` ni de escuchar eventos de scroll o resize en el hilo principal. El patrón común es: crear el observer con un callback → llamar a `observe(elemento)` → recibir notificaciones por lotes cuando se producen cambios → llamar a `disconnect()` al desmontar.

```js
// Patrón general de los tres observers
const observer = new AlgúnObserver(callback, opciones);
observer.observe(elemento);
// …cuando ya no se necesita:
observer.disconnect();
```

## Comparativa

| Observer | Qué observa | Cuándo usar |
|---|---|---|
| `IntersectionObserver` | Visibilidad de un elemento respecto al viewport o a un contenedor | Lazy loading, animaciones al entrar al viewport, analytics de visibilidad, infinite scroll |
| `MutationObserver` | Cambios en el árbol del DOM (nodos, atributos, texto) | Reaccionar a cambios de terceros, extensiones de browser, reactive sin frameworks |
| `ResizeObserver` | Cambios en el tamaño de un elemento específico | Componentes que se adaptan al contenedor, charts responsivos, container queries en JS |

## Notas de esta sección

- [[01 Intersection Observer]] — detectar cuándo un elemento entra o sale del viewport.
- [[02 Mutation Observer]] — observar cambios en nodos, atributos y texto del DOM.
- [[03 Resize Observer]] — detectar cambios de tamaño en elementos individuales.

## Por qué no usar scroll/resize listeners

El listener clásico `window.addEventListener("scroll", fn)` se dispara en cada frame durante el scroll (potencialmente 60 veces por segundo), ejecuta el callback en el hilo principal y puede forzar reflows si accede a propiedades de layout como `getBoundingClientRect()`. Los Observers delegan la detección al motor del browser (fuera del hilo principal en algunos casos) y entregan notificaciones por lotes, lo que los hace significativamente más eficientes.

> [!tip]
> Los tres Observers siguen el mismo patrón de ciclo de vida: `observe` → callbacks por lotes → `unobserve`/`disconnect`. Si el elemento que se observa se elimina del DOM, el observer deja de recibir eventos para él; llamar `disconnect()` explícitamente evita memory leaks cuando el observer mismo se descarta.

> [!warning]
> Los callbacks de los Observers no se ejecutan en el mismo tick que el cambio. `MutationObserver` entrega sus records como microtasks (antes del render); `IntersectionObserver` y `ResizeObserver` lo hacen antes del paint, pero fuera del microtask queue. No asumir ejecución síncrona.

## Notas relacionadas

- [[../05 Canvas API/index|Canvas API]] — `ResizeObserver` es el mecanismo correcto para adaptar el tamaño del canvas a su contenedor.
- [[../../06 Manejo de Eventos/index|Eventos]] — alternativa clásica basada en listeners de scroll/resize.
- [[../../07 Programación Asíncrona/index|Programación Asíncrona]] — los callbacks de Observers se ejecutan fuera del call stack sincrónico.
