---
title: Animaciones y Transiciones — Movimiento en la interfaz
aliases:
  - css animations
  - transitions
tags:
  - css
  - api/concepto
  - animacion
draft: false
---

# Animaciones y Transiciones

> [!definicion]
> CSS anima los cambios de estilo para dar **fluidez y vida** a la interfaz. Las [[01 Transiciones (transition)/index | transiciones]] interpolan suavemente entre dos estados (hover, foco); las [[04 Animaciones (@keyframes)/index | animaciones `@keyframes`]] definen secuencias más complejas; las [[03 Transformaciones (transform)/index | transformaciones]] mueven, rotan y escalan elementos de forma eficiente.

```css
.boton {
  transition: background 0.2s ease, transform 0.2s ease;
}
.boton:hover {
  background: #b48ef0;
  transform: translateY(-2px);
}
```

## Mapa de la sección

- [[01 Transiciones (transition)/index]] — interpolar entre dos estados.
- [[02 Funciones de Tiempo/index]] — las curvas de aceleración (`ease`, `cubic-bezier`…).
- [[03 Transformaciones (transform)/index]] — mover, rotar, escalar (2D y 3D).
- [[04 Animaciones (@keyframes)/index]] — secuencias con fotogramas.
- [[05 Rendimiento en Animaciones/index]] — animar de forma fluida (60fps).
- [[06 Animaciones de Scroll/index]] — animaciones ligadas al desplazamiento.

## Transición vs. animación

> [!info] Dos estados vs. una secuencia
> La diferencia esencial:
> - **Transición** (`transition`): interpola entre el estado **actual** y un estado **nuevo** disparado por un cambio (hover, una clase añadida por JS). Va de A a B. Simple y reactiva.
> - **Animación** (`@keyframes` + `animation`): define una **secuencia** de fotogramas que puede tener varios pasos, repetirse, ir y volver, y ejecutarse sin un disparador. Más potente y autónoma.
>
> Para un hover suave, transición; para un spinner que gira en bucle o una entrada elaborada, animación.

## La regla de oro del rendimiento

> [!warning] Anima transform y opacity, no layout
> El consejo más importante de toda la sección: para animaciones **fluidas a 60fps**, anima solo `transform` y `opacity`. Animar propiedades que cambian el **layout** (`width`, `height`, `top`, `margin`) o el **pintado** (`background`, `box-shadow`) es **mucho más caro** y causa tirones. `transform` y `opacity` los gestiona la GPU sin recalcular el layout. Detalle en [[03 Animar transform y opacity]].

## Respetar prefers-reduced-motion

> [!warning] Las animaciones deben ser opcionales
> Algunas personas sufren malestar con el movimiento. Toda animación decorativa debe respetar [[06 Preferencias (prefers-color-scheme, prefers-reduced-motion) | `prefers-reduced-motion`]]:
> ```css
> @media (prefers-reduced-motion: reduce) {
>   *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
> }
> ```
> Es un requisito de accesibilidad, no opcional.

## Notas relacionadas

- [[01 Transiciones (transition)/index]] — el punto de partida, lo más usado.
- [[03 Animar transform y opacity]] — el rendimiento.
- [[06 Preferencias (prefers-color-scheme, prefers-reduced-motion)]] — el respeto al usuario.
