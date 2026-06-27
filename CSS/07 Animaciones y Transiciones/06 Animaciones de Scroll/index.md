---
title: Animaciones de Scroll — Animaciones ligadas al desplazamiento
aliases:
  - scroll-driven animations
tags:
  - css
  - api/concepto
  - animacion
draft: false
order: 6
---

# Animaciones de Scroll

> [!definicion]
> Las **animaciones de scroll** (scroll-driven animations) ligan el progreso de una animación al **desplazamiento** de la página, en vez de al tiempo. La animación avanza a medida que el usuario hace scroll y retrocede si vuelve atrás. Antes requerían JavaScript; ahora CSS las hace de forma nativa y eficiente.

```css
.barra-progreso {
  animation: crecer linear;
  animation-timeline: scroll();   /* ligada al scroll, no al tiempo */
}
@keyframes crecer { to { transform: scaleX(1); } }
```

## Mapa de la subsección

- [[01 scroll-timeline]] — animaciones ligadas al scroll de un contenedor.
- [[02 view-timeline]] — animaciones según la **visibilidad** de un elemento.

## Del tiempo al scroll

> [!info] La línea de tiempo es el scroll, no el reloj
> En una animación normal, el "avance" lo marca el **tiempo** (`duration`). En una animación de scroll, lo marca la **posición del scroll**: al 0% de scroll, la animación está al inicio; al 100%, al final. La propiedad `animation-timeline` cambia la "línea de tiempo" del reloj a:
> - **`scroll()`**: el progreso de scroll de un contenedor ([[01 scroll-timeline]]).
> - **`view()`**: la visibilidad de un elemento en el viewport ([[02 view-timeline]]).

## Los dos casos de uso

> [!tip] Barra de progreso vs. revelar al entrar
> Los dos efectos más comunes:
> 1. **Barra de progreso de lectura**: una barra que se llena según cuánto has bajado en la página. Usa [[01 scroll-timeline | `scroll()`]].
> 2. **Revelar elementos al entrar en pantalla** (fade-in, deslizar): cada elemento se anima cuando entra en el viewport. Usa [[02 view-timeline | `view()`]]. Es el reemplazo nativo del clásico "animate on scroll" que antes necesitaba `IntersectionObserver` en JS.

## La ventaja sobre JavaScript

> [!info] Eficiente y fuera del hilo principal
> Hacer animaciones de scroll con JS (escuchando el evento `scroll`) es **costoso**: el evento se dispara constantemente y corre en el hilo principal, causando tirones. Las animaciones de scroll **nativas de CSS** corren fuera del hilo principal (en el compositor), así que son **fluidas** aunque la página esté ocupada. Es una mejora de rendimiento importante, además de menos código.

## Soporte y respaldo

> [!warning] Soporte aún creciendo
> Las animaciones de scroll nativas son **recientes**; su soporte está creciendo pero aún no es universal. Conviene usarlas como **mejora progresiva**: el contenido debe funcionar sin ellas (los elementos visibles, la barra ausente), y la animación se añade donde hay soporte. Comprueba con `@supports (animation-timeline: scroll())`.

## Notas relacionadas

- [[01 scroll-timeline]] — ligar al scroll de un contenedor.
- [[02 view-timeline]] — ligar a la visibilidad de un elemento.
- [[03 Animar transform y opacity]] — qué animar en estos efectos.
