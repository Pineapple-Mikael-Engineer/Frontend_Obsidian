---
title: Transformaciones (transform) — Mover, rotar, escalar, deformar
aliases:
  - transform
tags:
  - css
  - api/concepto
  - animacion
draft: false
order: 3
---

# Transformaciones (transform)

> [!definicion]
> La propiedad `transform` aplica transformaciones geométricas a un elemento: **trasladar** (mover), **rotar**, **escalar** y **deformar** (skew), en 2D o 3D. Es fundamental no solo por sus efectos, sino porque es la forma **más eficiente** de mover y animar elementos (la GPU la gestiona sin recalcular el layout).

```css
.x { transform: translateX(20px) rotate(15deg) scale(1.1); }
```

## Mapa de la subsección

- [[01 Transformaciones 2D (translate, rotate, scale, skew)]] — las cuatro básicas.
- [[02 matrix()]] — todas combinadas en una matriz.
- [[03 Transformaciones 3D (translateZ, rotateX, scale3d)]] — el espacio tridimensional.
- [[04 perspective y perspective-origin]] — la profundidad 3D.
- [[05 transform-origin]] — el punto sobre el que se transforma.
- [[06 transform-style]] — 3D anidado.
- [[07 backface-visibility]] — la cara trasera al voltear.

## La gran ventaja: rendimiento

> [!tip] transform no provoca reflow
> A diferencia de mover un elemento con `top`/`left`/`margin` (que recalculan el layout de toda la página, un **reflow** caro), `transform` mueve el elemento en la fase de **composición**, gestionada por la **GPU**, sin tocar el layout. Por eso:
> - Animar `transform: translateX()` es **fluido** (60fps); animar `left` es **lento** (tirones).
> - Es la propiedad preferida para animaciones, junto a `opacity`.
>
> Detalle en [[03 Animar transform y opacity]].

## Las funciones se encadenan (y el orden importa)

> [!warning] El orden de las funciones cambia el resultado
> En `transform`, las funciones se aplican **de derecha a izquierda** sobre el elemento, y el orden **importa**: rotar y luego trasladar no es lo mismo que trasladar y luego rotar (la traslación ocurre en el sistema de coordenadas ya rotado):
> ```css
> transform: rotate(45deg) translateX(100px);   /* mueve en diagonal (sistema rotado) */
> transform: translateX(100px) rotate(45deg);   /* mueve a la derecha, luego rota */
> ```
> Si los resultados no son los esperados, prueba a invertir el orden.

## Las propiedades individuales modernas

> [!tip] translate, rotate, scale como propiedades sueltas
> CSS moderno permite las transformaciones como **propiedades independientes** (`translate`, `rotate`, `scale`), además del shorthand `transform`. Esto facilita animarlas por separado sin pisarse:
> ```css
> .x { translate: 20px 0; rotate: 15deg; scale: 1.1; }
> ```
> Útil cuando distintos estados/animaciones tocan distintas transformaciones (una anima la rotación, otra la escala) sin sobrescribirse, algo imposible con el `transform` único.

## transform no afecta al layout

Un elemento transformado **no desplaza** a los demás: ocupa su sitio original en el flujo, y la transformación es puramente visual (como una capa por encima). Por eso un `scale(1.5)` puede solaparse con vecinos sin empujarlos.

## Notas relacionadas

- [[01 Transformaciones 2D (translate, rotate, scale, skew)]] — el punto de partida.
- [[03 Animar transform y opacity]] — por qué es la propiedad para animar.
- [[05 transform-origin]] — el punto de pivote.
