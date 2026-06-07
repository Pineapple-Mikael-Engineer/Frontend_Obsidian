---
title: scroll-timeline — Animación ligada al scroll de un contenedor
aliases:
  - scroll-timeline
  - "scroll()"
tags:
  - css
  - api/propiedad
  - animacion
propiedad: animation-timeline
grupo: animacion
valor_inicial: auto
hereda: false
animable: false
draft: false
---

# scroll-timeline

> [!definicion]
> Una **scroll timeline** liga el progreso de una animación al **desplazamiento** de un contenedor (o de la página): la animación va del 0% al 100% según el scroll va del principio al final. El caso de uso estrella es la **barra de progreso de lectura**.

```css
.progreso {
  animation: llenar linear;
  animation-timeline: scroll(root block);
}
@keyframes llenar { from { transform: scaleX(0); } to { transform: scaleX(1); } }
```

## La función scroll()

> [!info] scroll() define qué contenedor y qué eje
> `animation-timeline: scroll(<contenedor> <eje>)` liga la animación al scroll:
> - **Contenedor**: `root` (la página), `nearest` (el ancestro con scroll más cercano), `self`.
> - **Eje**: `block` (vertical, lo normal), `inline` (horizontal).
> ```css
> animation-timeline: scroll(root block);   /* el scroll vertical de la página */
> ```

## La barra de progreso de lectura

> [!tip] El efecto clásico, ahora sin JavaScript
> Una barra fija en lo alto de la página que se llena según cuánto has bajado:
> ```css
> .barra-lectura {
>   position: fixed; top: 0; left: 0;
>   height: 4px; width: 100%;
>   background: #cba6f7;
>   transform-origin: left;
>   transform: scaleX(0);
>   animation: llenar linear;
>   animation-timeline: scroll(root block);
> }
> @keyframes llenar { to { transform: scaleX(1); } }
> ```
> La barra crece de 0 a 100% del ancho a medida que se hace scroll, ligada al progreso de la página. Antes esto requería `IntersectionObserver`/eventos de scroll en JS; ahora es CSS puro y eficiente.

## timeline linear

> [!warning] Las animaciones de scroll suelen ir en linear
> Como el "avance" lo da el scroll (que el usuario controla a su ritmo), la `animation-timing-function` casi siempre es `linear`: el progreso debe seguir **fielmente** la posición del scroll, sin acelerar ni frenar. Un `ease` haría que la barra avanzara de forma desincronizada con el scroll real.

## scroll() vs. view()

> [!info] El scroll del contenedor vs. la visibilidad del elemento
> - **`scroll()`**: liga al scroll **global** del contenedor (cuánto has bajado). Para barras de progreso de toda la página.
> - **[[02 view-timeline | `view()`]]**: liga a la **visibilidad** de un elemento concreto (cuánto ha entrado en pantalla). Para revelar elementos al aparecer.
>
> Para una barra de "cuánto llevas leído", `scroll()`; para "anima esta tarjeta cuando entre en pantalla", `view()`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `scroll(root block)` para barras de progreso de lectura de la página.
> - Mantén `linear`: el progreso debe seguir el scroll fielmente.
> - Anima `transform: scaleX()` (eficiente) para la barra, no `width`.
> - Trátalo como mejora progresiva: comprueba el soporte con `@supports`.

## Errores comunes

> [!warning] Trampas
> - **Usar `ease`** en vez de `linear`: el progreso se desincroniza del scroll.
> - **Animar `width`** en vez de `transform: scaleX()`: menos eficiente.
> - **Depender de ello sin respaldo**: el soporte aún crece.

## Notas relacionadas

- [[02 view-timeline]] — ligar a la visibilidad de un elemento.
- [[index]] — el concepto de animaciones de scroll.
- [[01 Transformaciones 2D (translate, rotate, scale, skew)]] — `scaleX` para la barra.
