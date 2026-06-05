---
title: Unidades de Medida — Expresar longitudes y tamaños
aliases:
  - css units
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Unidades de Medida

> [!definicion]
> Las **unidades** expresan longitudes en CSS: tamaños, espaciados, posiciones. Se dividen en **absolutas** (un tamaño fijo, como `px`), **relativas** (a la fuente, al viewport o al contenedor) y **funcionales** (`calc()`, `clamp()`, que combinan unidades). Elegir bien la unidad es la base de un diseño escalable y accesible.

```css
font-size: 16px;           /* absoluta */
padding: 1rem;             /* relativa a la fuente raíz */
width: 50vw;               /* relativa al viewport */
width: clamp(20rem, 50%, 40rem);   /* funcional */
```

## Familias de unidades

| Familia | Relativa a… | Ejemplos | Nota |
|---------|-------------|----------|------|
| Absolutas | Nada (tamaño fijo) | `px`, `pt`, `cm` | [[01 Unidades Absolutas (px, pt, cm)]] |
| Relativas a fuente | El `font-size` | `em`, `rem`, `ch`, `ex` | [[02 Relativas a Fuente (em, rem, ch, ex)]] |
| Relativas al viewport | El tamaño de la ventana | `vw`, `vh`, `vmin`, `vmax` | [[03 Relativas al Viewport (vw, vh, vmin, vmax)]] |
| Viewport dinámico | La ventana real en móvil | `svh`, `lvh`, `dvh` | [[04 Viewport Dinámico (svh, lvh, dvh)]] |
| Porcentajes | El contenedor | `%` | [[05 Porcentajes]] |
| Funcionales | Combinan unidades | `calc()`, `min()`, `max()`, `clamp()` | [[06 calc()]], [[07 min(), max(), clamp()]] |

## El eje absoluto vs. relativo

> [!info] Lo relativo escala; lo absoluto no
> La distinción más importante:
> - Una unidad **absoluta** (`px`) mide siempre lo mismo, sin importar el contexto ni las preferencias del usuario.
> - Una unidad **relativa** (`rem`, `%`, `em`) se **adapta**: si el usuario aumenta el tamaño de fuente del navegador, los tamaños en `rem` crecen con él; los `px`, no.
>
> Por eso el consejo general es **relativo para tipografía y espaciado** (accesible y escalable), reservando `px` para detalles que deben ser fijos (un borde de 1px, un radio pequeño).

## La regla práctica por contexto

| Para… | Usa preferentemente |
|--------|---------------------|
| Tamaño de fuente | `rem` (escala con la preferencia del usuario) |
| Espaciado (padding, margin, gap) | `rem` |
| Anchos de layout | `%`, `fr` (grid), `ch`, con `max-width` en `rem` |
| Altura de "pantalla completa" | `dvh` (móvil) o `vh` |
| Bordes finos, radios pequeños | `px` |
| Tipografía fluida | `clamp()` |

## Notas relacionadas

- [[02 Relativas a Fuente (em, rem, ch, ex)]] — `rem`/`em`, las más usadas.
- [[07 min(), max(), clamp()]] — el diseño fluido moderno.
- [[06 box-sizing]] — cómo se interpretan los tamaños en el modelo de caja.
