---
title: Rendimiento en Animaciones — Animar a 60fps
aliases:
  - animation performance
tags:
  - css
  - api/concepto
  - animacion
draft: false
order: 5
---

# Rendimiento en Animaciones

> [!definicion]
> No todas las propiedades cuestan lo mismo de animar. Una animación fluida corre a **60fps** (un fotograma cada ~16ms); si el navegador no puede recalcular todo a tiempo, se producen **tirones** (jank). La clave del rendimiento es entender qué provoca cada propiedad: **layout**, **paint** o solo **composición**.

```css
/* ✅ fluido: solo composición (GPU) */
.x { transform: translateX(100px); }
/* ❌ caro: provoca reflow de toda la página */
.x { left: 100px; }
```

## Mapa de la subsección

- [[01 Layout, Paint y Composición]] — las tres fases del renderizado y su coste.
- [[02 will-change]] — avisar al navegador de qué se va a animar.
- [[03 Animar transform y opacity]] — la regla de oro del rendimiento.

## La regla de oro

> [!tip] Anima solo transform y opacity
> El consejo más importante de toda la sección de animaciones, repetido por una razón: **anima solo `transform` y `opacity`**. Son las dos únicas propiedades que el navegador puede animar en la fase de **composición** (gestionada por la GPU), sin recalcular el layout ni repintar. Cualquier otra propiedad (`width`, `top`, `margin`, `background`, `box-shadow`) es **más cara** y puede causar tirones, sobre todo en móvil. Detalle en [[03 Animar transform y opacity]].

## Por qué importa tanto

> [!info] 16 milisegundos por fotograma
> A 60fps, el navegador tiene **~16ms** para producir cada fotograma. Si una animación lo obliga a recalcular el layout de toda la página (reflow) en cada uno de esos fotogramas, no llega a tiempo y la animación va a **saltos**. Animar `transform`/`opacity` evita el reflow: el navegador solo recompone capas ya pintadas, algo que la GPU hace casi gratis. Por eso una animación de `translateX` es suave y una de `left` no.

## Notas relacionadas

- [[03 Animar transform y opacity]] — la regla práctica.
- [[01 Layout, Paint y Composición]] — el porqué técnico.
- [[03 Transformaciones (transform)/index]] — `transform`, la propiedad estrella.
