---
title: Unidades vw para Fuente — Escalar el texto con el viewport
aliases:
  - vw font-size
tags:
  - css
  - api/concepto
  - responsive
draft: false
---

# Unidades vw para Fuente

> [!definicion]
> Usar la unidad **`vw`** en `font-size` hace que el texto **escale con el ancho del viewport**: `font-size: 4vw` significa "el 4% del ancho de la ventana". Es la base de la tipografía fluida, pero usada **sola** tiene problemas serios que [[02 clamp() para Tipografía Fluida | `clamp()`]] resuelve.

```css
h1 { font-size: 5vw; }   /* crece con la ventana... sin límites */
```

## Cómo escala

`1vw` = 1% del ancho del viewport. Con `font-size: 5vw`:
- En una ventana de 400px → 20px.
- En una de 1200px → 60px.
- En una de 2000px → 100px.

El texto crece **proporcionalmente** al ancho, de forma continua.

## Los dos problemas de vw puro

> [!warning] Sin límites y sin respeto al zoom
> Usar `vw` solo en `font-size` tiene dos defectos graves:
> 1. **Sin límites**: el texto se vuelve **diminuto** en pantallas muy estrechas y **gigante** en monitores enormes. `5vw` son 100px en un monitor 4K (ilegiblemente grande) y 16px en un móvil pequeño (quizá demasiado pequeño para un titular).
> 2. **Ignora el zoom de texto**: las unidades de viewport **no responden** a la preferencia de tamaño de fuente del usuario ni a su zoom de texto, una **barrera de accesibilidad** (el usuario que sube el tamaño no ve crecer el texto en `vw`).

```css
/* ❌ vw puro: sin límites, ignora el zoom */
h1 { font-size: 5vw; }
```

## La solución: combinar con rem y acotar

> [!tip] vw siempre acompañado de rem y límites
> `vw` rara vez se usa solo. Se combina con:
> - Una base en **`rem`** (que sí respeta el zoom): `1rem + 2vw`.
> - **Límites** con `clamp()`: un mínimo y un máximo.
> ```css
> h1 { font-size: clamp(1.75rem, 1rem + 4vw, 3.5rem); }
> ```
> La parte `1rem` garantiza un mínimo legible y respeta el zoom; el `4vw` aporta la fluidez; `clamp()` acota los extremos. Detalle en [[02 clamp() para Tipografía Fluida]].

## Cuándo vw sola es aceptable

`vw` puro puede usarse en casos muy controlados donde el texto **debe** escalar exactamente con la pantalla y no hay riesgo de extremos: un titular de portada de pantalla completa con un rango de tamaños conocido y limitado. Incluso ahí, `clamp()` suele ser más seguro.

## Buenas prácticas

> [!tip] Recomendaciones
> - **No** uses `vw` solo en `font-size`: combínalo con `rem` y acótalo con `clamp()`.
> - Incluye una base en `rem` para respetar el zoom del usuario (accesibilidad).
> - Reserva el `vw` puro para casos muy controlados (un hero a pantalla completa).
> - Piensa siempre en los extremos: ¿cómo se ve en un móvil mínimo y en un 4K?

## Errores comunes

> [!warning] Trampas
> - **`vw` puro**: texto diminuto o gigante en los extremos.
> - **Ignorar el zoom**: el `vw` no responde a la preferencia del usuario.
> - **Sin `rem` en la fórmula**: pierde la base accesible.

## Notas relacionadas

- [[02 clamp() para Tipografía Fluida]] — la forma correcta de usar `vw`.
- [[03 Relativas al Viewport (vw, vh, vmin, vmax)]] — las unidades de viewport.
- [[02 Relativas a Fuente (em, rem, ch, ex)]] — `rem`, la base accesible.
