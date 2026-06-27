---
title: Relativas al Viewport — vw, vh, vmin, vmax
aliases:
  - viewport units
  - vw
  - vh
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 3
---

# Relativas al Viewport (vw, vh, vmin, vmax)

> [!definicion]
> Las unidades de **viewport** se calculan como un porcentaje del tamaño de la **ventana** del navegador. `1vw` es el 1% del ancho del viewport; `1vh`, el 1% del alto. Permiten dimensionar elementos en relación con la pantalla, no con el contenedor.

```css
.hero   { height: 100vh; }      /* alto = toda la altura de la ventana */
.titulo { font-size: 5vw; }     /* tamaño = 5% del ancho de la ventana */
```

## Las cuatro unidades

| Unidad | Equivale a | Útil para |
|--------|-----------|-----------|
| `vw` | 1% del **ancho** del viewport | Anchos y tipografía fluida |
| `vh` | 1% del **alto** del viewport | Secciones de pantalla completa |
| `vmin` | 1% de la dimensión **menor** | Tamaños que caben en ambos ejes |
| `vmax` | 1% de la dimensión **mayor** | Cubrir siempre la dimensión grande |

```css
/* Un cuadrado que siempre cabe en pantalla, oriente como oriente */
.cuadrado { width: 80vmin; height: 80vmin; }
```

## Casos de uso

| Patrón | Unidad |
|--------|--------|
| Sección "pantalla completa" (hero) | `100vh` (mejor `100dvh`, ver abajo) |
| Título que crece con la ventana | `vw` (con `clamp()`) |
| Elemento cuadrado que cabe en pantalla | `vmin` |
| Fondo que cubre la dimensión mayor | `vmax` |

## El problema de 100vh en móvil

> [!warning] 100vh "salta" en móviles
> El gran defecto histórico de `vh`: en móviles, la barra de direcciones del navegador **aparece y desaparece** al hacer scroll, cambiando el alto visible. Pero `100vh` se calcula contra el viewport **grande** (sin barras), así que una sección con `height: 100vh` queda **más alta** que la pantalla visible, cortando contenido o causando saltos. La solución son las [[04 Viewport Dinámico (svh, lvh, dvh) | unidades de viewport dinámico]] (`dvh`), que sí siguen el alto real. Para "pantalla completa" en móvil, usa `100dvh`, no `100vh`.

## vw y el scroll horizontal

> [!warning] 100vw puede causar scroll horizontal
> `100vw` incluye el ancho de la **barra de scroll vertical** en algunos navegadores de escritorio, así que un elemento de `width: 100vw` puede ser unos píxeles más ancho que el área de contenido, provocando un scroll horizontal molesto. Para "ancho completo", `width: 100%` suele ser más seguro que `100vw`.

## Tipografía fluida con vw

`vw` permite que un tamaño de fuente crezca con la ventana, pero sin límites se vuelve enorme o diminuto en los extremos. Por eso casi siempre se combina con [[07 min(), max(), clamp() | `clamp()`]]:

```css
/* crece con la ventana, pero acotado entre 1.5rem y 3rem */
h1 { font-size: clamp(1.5rem, 5vw, 3rem); }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `vh`/`vw` para dimensiones ligadas a la pantalla (heroes, fondos).
> - Para "pantalla completa" en móvil, prefiere `dvh` a `vh`.
> - Para "ancho completo", `width: 100%` suele ser más seguro que `100vw`.
> - Combina `vw` con `clamp()` para tipografía fluida acotada.
> - `vmin`/`vmax` para elementos que deben encajar en ambos ejes.

## Errores comunes

> [!warning] Trampas
> - **`100vh` en móvil**: queda más alto que la pantalla visible.
> - **`100vw`**: posible scroll horizontal por la barra de scroll.
> - **`vw` puro en tipografía** sin `clamp()`: tamaños extremos en pantallas muy grandes o pequeñas.

## Notas relacionadas

- [[04 Viewport Dinámico (svh, lvh, dvh)]] — la solución al problema de `100vh`.
- [[07 min(), max(), clamp()]] — acotar las unidades de viewport.
- [[02 clamp() para Tipografía Fluida]] — tipografía responsiva con `vw`.
