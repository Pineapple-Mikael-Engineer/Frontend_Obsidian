---
title: Viewport Dinámico — svh, lvh, dvh
aliases:
  - dynamic viewport units
  - dvh
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 4
---

# Viewport Dinámico (svh, lvh, dvh)

> [!definicion]
> Estas unidades resuelven el problema de `100vh` en móvil. En lugar de un alto fijo, miden el viewport teniendo en cuenta que las **barras del navegador** (dirección, herramientas) aparecen y desaparecen: `svh` usa el viewport **pequeño** (con barras), `lvh` el **grande** (sin barras) y `dvh` el **dinámico** (el actual, que cambia al hacer scroll).

```css
.pantalla-completa { min-height: 100dvh; }   /* alto real, sin saltos */
```

## Las tres variantes (y sus equivalentes de ancho)

| Unidad | Mide el viewport… | Cuándo |
|--------|-------------------|--------|
| `svh` / `svw` | **Small**: con las barras visibles | El menor alto posible |
| `lvh` / `lvw` | **Large**: con las barras ocultas | El mayor alto posible |
| `dvh` / `dvw` | **Dynamic**: el actual, en tiempo real | Se ajusta al estado de las barras |

(También existen `svmin`, `lvmax`, etc., siguiendo el mismo patrón.)

## El problema que resuelven

> [!info] Por qué 100vh fallaba
> En móvil, la barra de direcciones ocupa espacio al cargar y se **oculta** al hacer scroll, agrandando el área visible. `vh` (el clásico) se calcula contra el viewport **grande** (sin barra), así que `100vh` es más alto que lo visible al inicio, cortando contenido bajo la barra. Las unidades dinámicas dan el alto **real**:
> - `100svh` → cabe siempre, incluso con la barra visible (puede dejar hueco al ocultarse).
> - `100lvh` → ocupa el máximo (puede cortar mientras la barra está visible).
> - `100dvh` → se ajusta en vivo al estado actual.

## dvh: el más usado, con matiz

> [!tip] dvh para "pantalla completa", con cuidado con el reflujo
> `100dvh` es la mejor opción para una sección que debe ocupar exactamente la pantalla en móvil: se ajusta al área visible real. El **matiz**: como cambia al aparecer/ocultar la barra, un elemento con `height: 100dvh` se **redimensiona** durante el scroll, lo que puede provocar reflujos visibles. Para muchos casos, `svh` (estable, el menor) o `min-height: 100svh` evitan el "salto" a costa de dejar a veces un poco de hueco. La elección depende del efecto deseado.

## Recetas

```css
/* Hero que llena la pantalla en móvil sin cortar */
.hero { min-height: 100dvh; }

/* Modal a pantalla completa estable (sin reflujo al scroll) */
.modal { height: 100svh; }
```

## Soporte y respaldo

> [!info] Da un respaldo vh
> Las unidades dinámicas tienen buen soporte moderno, pero para navegadores antiguos conviene un respaldo con `vh` antes (el navegador usa la última que entiende):
> ```css
> .hero {
>   min-height: 100vh;    /* respaldo */
>   min-height: 100dvh;   /* moderno */
> }
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Para "pantalla completa" en móvil, usa `dvh`/`svh` en lugar de `vh`.
> - `dvh` si quieres seguir el área visible real; `svh` si prefieres estabilidad sin reflujo.
> - Da un respaldo en `vh` para navegadores antiguos.
> - Usa `min-height` en vez de `height` para no cortar contenido más alto.

## Errores comunes

> [!warning] Trampas
> - **Seguir usando `100vh`** en móvil: corta contenido bajo la barra.
> - **`height: 100dvh`** donde el reflujo durante el scroll molesta (usa `svh`).
> - **Sin respaldo** en navegadores que no soportan las dinámicas.

## Notas relacionadas

- [[03 Relativas al Viewport (vw, vh, vmin, vmax)]] — las clásicas y su problema en móvil.
- [[01 meta viewport]] — el prerrequisito del comportamiento del viewport.
- [[index]] — el panorama de unidades.
