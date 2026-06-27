---
title: backface-visibility — La cara trasera al voltear
aliases:
  - backface-visibility
tags:
  - css
  - api/propiedad
  - animacion
propiedad: backface-visibility
grupo: animacion
valor_inicial: visible
hereda: false
animable: false
draft: false
order: 7
---

# backface-visibility

> [!definicion]
> `backface-visibility` decide si la **cara trasera** de un elemento es visible cuando se voltea con una transformación 3D. `hidden` oculta el elemento cuando da la espalda al espectador, esencial para tarjetas que se voltean (que no deben mostrar su contenido "espejado").

```css
.cara { backface-visibility: hidden; }
```

## Valores

| Valor | Comportamiento |
|-------|----------------|
| `visible` | La cara trasera se ve (espejada) cuando da la espalda (por defecto) |
| `hidden` | La cara trasera es **invisible** cuando da la espalda |

## El problema que resuelve

> [!info] El contenido espejado al voltear
> Cuando un elemento gira más de 90° (`rotateY(180deg)`), muestra su **reverso**: el contenido aparece **espejado** (invertido como en un espejo). En una flip card, no quieres ver el frente reflejado, sino la cara **trasera**. `backface-visibility: hidden` oculta cada cara cuando da la espalda, dejando ver solo la que mira al frente:
> ```css
> .cara-frontal, .cara-trasera {
>   backface-visibility: hidden;   /* cada una invisible cuando da la espalda */
> }
> ```

## La flip card completa

> [!tip] Las tres piezas de una tarjeta que se voltea
> Una flip card necesita la combinación de tres propiedades 3D:
> ```css
> .escena { perspective: 1000px; }                    /* 1. profundidad */
> .tarjeta {
>   transform-style: preserve-3d;                     /* 2. mantener el 3D */
>   transition: transform 0.6s;
> }
> .escena:hover .tarjeta { transform: rotateY(180deg); }
> .cara-frontal, .cara-trasera {
>   position: absolute; inset: 0;
>   backface-visibility: hidden;                      /* 3. ocultar la cara que da la espalda */
> }
> .cara-trasera { transform: rotateY(180deg); }       /* la trasera ya está girada */
> ```
> - La frontal se ve de inicio (mira al frente).
> - La trasera está pre-girada 180°, así que cuando la tarjeta gira 180°, queda de frente.
> - `backface-visibility: hidden` evita ver el frente espejado durante el giro.

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon `backface-visibility: hidden` en **ambas** caras de una flip card.
> - Combínalo con `perspective` (escena) y `preserve-3d` (tarjeta).
> - Pre-gira la cara trasera 180° para que quede de frente al voltear.
> - Sin él, verías el contenido espejado al pasar de 90°.

## Errores comunes

> [!warning] Trampas
> - **Contenido espejado** visible por no poner `backface-visibility: hidden`.
> - **Olvidarlo en una de las dos caras**: una se ve espejada.
> - **Sin `preserve-3d`** en el padre: el efecto de volteo no funciona.

## Notas relacionadas

- [[03 Transformaciones 3D (translateZ, rotateX, scale3d)]] — el `rotateY` que voltea.
- [[06 transform-style]] — `preserve-3d`, imprescindible para la flip card.
- [[04 perspective y perspective-origin]] — la perspectiva de la escena.
