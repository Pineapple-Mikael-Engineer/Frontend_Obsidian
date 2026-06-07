---
title: Animar transform y opacity — La regla de oro
aliases:
  - animate transform opacity
tags:
  - css
  - api/concepto
  - animacion
draft: false
---

# Animar transform y opacity

> [!definicion]
> La regla más importante del rendimiento de animaciones: **anima solo `transform` y `opacity`**. Son las dos únicas propiedades que el navegador anima en la fase de **composición** (GPU), sin recalcular el layout ni repintar. Animar cualquier otra cosa es más caro y puede causar tirones.

```css
/* ✅ fluido a 60fps */
.x { transition: transform 0.3s, opacity 0.3s; }
/* ❌ tirones */
.x { transition: left 0.3s, width 0.3s; }
```

## Por qué solo estas dos

> [!info] Saltan layout y paint
> `transform` y `opacity` son especiales: el navegador las puede aplicar **sin** recalcular dónde van los elementos (layout) ni volver a pintar sus píxeles (paint). Solo recompone capas ya pintadas, algo que la **GPU** hace casi gratis. Por eso son las únicas que se animan de forma garantizadamente fluida. El resto de propiedades dispara fases caras (ver [[01 Layout, Paint y Composición]]).

## La tabla de traducción

> [!tip] Reemplazar propiedades caras por transform
> Casi cualquier efecto que se haría con una propiedad de layout tiene un equivalente con `transform`:
> | En vez de animar… | Anima… |
> |-------------------|--------|
> | `left` / `top` / `margin` | `transform: translate()` |
> | `width` / `height` | `transform: scale()` |
> | (girar) | `transform: rotate()` |
> | mostrar/ocultar | `opacity` |
> | `box-shadow` (elevación) | `opacity` de una sombra en pseudoelemento |
>
> Repensar el efecto en términos de `transform`/`opacity` es la habilidad clave para animaciones fluidas.

## Ejemplos del antes y después

```css
/* ❌ Mover con left (reflow) */
@keyframes mal { from { left: 0; } to { left: 200px; } }
/* ✅ Mover con transform (composición) */
@keyframes bien { from { transform: translateX(0); } to { transform: translateX(200px); } }

/* ❌ Crecer con width (reflow) */
.card:hover { width: 320px; }
/* ✅ Crecer con scale (composición) */
.card:hover { transform: scale(1.05); }
```

## El truco de la sombra animada

> [!tip] Animar la opacidad de una sombra, no la sombra
> Animar `box-shadow` directamente (para un efecto de elevación al pasar el ratón) es **caro** (repaint). El truco eficiente: poner la sombra "grande" en un **pseudoelemento** con `opacity: 0`, y animar solo su **opacidad**:
> ```css
> .card { position: relative; }
> .card::after {
>   content: ""; position: absolute; inset: 0;
>   box-shadow: 0 12px 24px rgb(0 0 0 / 0.2);
>   opacity: 0; transition: opacity 0.3s;
> }
> .card:hover::after { opacity: 1; }   /* la sombra "aparece" sin repintar */
> ```

## scale puede deformar el contenido

> [!warning] El compromiso de scale
> `transform: scale()` es eficiente pero **escala todo** el elemento, incluido su contenido (texto, bordes), que se puede ver borroso o deformado a tamaños grandes. Para un crecimiento sutil (1.02–1.1) no se nota; para cambios grandes, a veces hay que aceptar el coste de animar la propiedad real o usar técnicas más avanzadas (FLIP). Es un compromiso entre rendimiento y fidelidad.

## Buenas prácticas

> [!tip] Recomendaciones
> - Anima **solo** `transform` y `opacity` siempre que puedas.
> - Reemplaza `left`/`top`/`width` por `translate`/`scale`.
> - Para sombras animadas, anima la opacidad de una sombra en pseudoelemento.
> - Respeta `prefers-reduced-motion` en toda animación decorativa.
> - Mide con DevTools si dudas del coste.

## Errores comunes

> [!warning] Trampas
> - **Animar `left`/`top`/`width`/`margin`**: reflow, tirones.
> - **Animar `box-shadow`** directamente: repaint costoso.
> - **`scale` grande** que emborrona el contenido.

## Notas relacionadas

- [[01 Layout, Paint y Composición]] — el porqué técnico.
- [[02 will-change]] — el ajuste fino para casos concretos.
- [[01 Transformaciones 2D (translate, rotate, scale, skew)]] — las funciones a usar.
