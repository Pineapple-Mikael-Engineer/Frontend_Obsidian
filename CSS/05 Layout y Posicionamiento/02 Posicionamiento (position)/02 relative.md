---
title: position relative — Desplazar y crear referencia
aliases:
  - position relative
tags:
  - css
  - api/propiedad
  - layout
propiedad: position
grupo: layout
valor_inicial: static
hereda: false
animable: false
draft: false
order: 2
---

# position relative

> [!definicion]
> `position: relative` posiciona el elemento **respecto a su propia posición original**: se desplaza con `top`/`left`… **sin** salir del flujo (deja su hueco original reservado). Su uso más importante no es desplazar, sino servir de **referencia** para hijos [[03 absolute | `absolute`]].

```css
.tarjeta { position: relative; }   /* referencia para hijos absolute */
.nudge   { position: relative; top: 2px; }   /* ajuste fino de posición */
```

## Las dos funciones de relative

> [!info] Desplazar (raro) y anclar (lo importante)
> `relative` hace dos cosas, y la segunda es la que más se usa:
> 1. **Desplazar el elemento** con `top`/`right`/`bottom`/`left` desde su posición original, **sin afectar** a los demás (su hueco original se conserva, así que puede solaparse con vecinos).
> 2. **Crear un contexto de posicionamiento**: se convierte en la referencia de sus descendientes `absolute`. Esta es su función estrella.

## Deja su hueco (no afecta al layout)

> [!warning] El desplazamiento de relative es "visual"
> Cuando desplazas un elemento con `relative` + `top`/`left`, el elemento se **mueve visualmente** pero su **hueco original permanece** reservado en el flujo. Los demás elementos no se reajustan: el espacio donde "estaba" sigue ocupado, y el elemento puede solaparse con sus vecinos:
> ```css
> .x { position: relative; top: 30px; }   /* baja 30px, pero deja un hueco arriba */
> ```
> Por eso `relative` no sirve para mover elementos en el layout (eso es Flexbox/Grid); su desplazamiento es para ajustes finos puntuales.

## El patrón relative + absolute

> [!tip] El uso que justifica relative
> El 90% de las veces, `relative` se pone en un contenedor **sin** desplazarlo, solo para que sus hijos `absolute` se anclen a él:
> ```css
> .producto { position: relative; }
> .producto .descuento {
>   position: absolute; top: 0.5rem; left: 0.5rem;   /* anclado a .producto */
> }
> ```
> Sin el `relative` en `.producto`, el badge se anclaría a un ancestro lejano o al viewport. Es el cimiento de badges, tooltips, overlays sobre tarjetas.

## Crea contexto de apilamiento (a veces)

`relative` con un `z-index` distinto de `auto` crea un [[07 z-index y Contexto de Apilamiento | contexto de apilamiento]], lo que afecta a cómo se superponen sus hijos. Sin `z-index`, no lo crea.

## Buenas prácticas

> [!tip] Recomendaciones
> - El uso principal: poner `relative` (sin desplazamiento) en un padre para anclar hijos `absolute`.
> - Para mover elementos en el layout, usa Flexbox/Grid, no el desplazamiento de `relative`.
> - Usa el desplazamiento de `relative` solo para ajustes finos (un par de píxeles).
> - Recuerda que el hueco original se conserva (puede solaparse con vecinos).

## Errores comunes

> [!warning] Trampas
> - **Mover elementos del layout con `relative`**: deja huecos y solapamientos.
> - **Olvidar el `relative` en el padre** y que el hijo `absolute` se ancle mal.
> - **Esperar que el desplazamiento reajuste** a los vecinos: no lo hace.

## Notas relacionadas

- [[03 absolute]] — el hijo que `relative` ancla.
- [[06 Desplazamiento (top, right, bottom, left)]] — las propiedades de desplazamiento.
- [[07 z-index y Contexto de Apilamiento]] — el apilado.
