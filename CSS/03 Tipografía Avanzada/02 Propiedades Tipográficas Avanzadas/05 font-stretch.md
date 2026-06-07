---
title: font-stretch — Anchura de la fuente (condensada/expandida)
aliases:
  - font-stretch
  - font-width
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: font-stretch
grupo: tipografia
valor_inicial: normal
hereda: true
animable: true
draft: false
---

# font-stretch

> [!definicion]
> `font-stretch` selecciona una variante **condensada o expandida** de la fuente: letras más estrechas o más anchas de lo normal. Funciona si la fuente incluye esas variantes (estáticas) o un eje de ancho `wdth` (variable). Su nombre moderno equivalente es `font-width`.

```css
.condensado { font-stretch: 75%; }      /* más estrecho */
.expandido  { font-stretch: 125%; }     /* más ancho */
```

## Valores

| Valor | Anchura |
|-------|---------|
| Porcentaje | `50%`–`200%` (`100%` = normal) |
| `ultra-condensed` | 50% |
| `condensed` | 75% |
| `normal` | 100% |
| `expanded` | 125% |
| `ultra-expanded` | 200% |

Las palabras clave equivalen a porcentajes concretos; el porcentaje da control fino (útil con fuentes variables).

## No es lo mismo que transform: scaleX

> [!warning] font-stretch usa diseños reales, no estira
> Una distinción crucial: `font-stretch` selecciona una variante de la fuente **diseñada** para ser más estrecha/ancha (las proporciones de las letras se mantienen correctas). En cambio, `transform: scaleX(0.75)` **deforma** las letras estirándolas geométricamente, lo que adelgaza los trazos verticales de forma antinatural y se ve mal:
> ```css
> .bien { font-stretch: condensed; }      /* diseño real condensado */
> .mal  { transform: scaleX(0.75); }       /* letras deformadas */
> ```

## Requiere soporte de la fuente

`font-stretch` solo tiene efecto si la fuente ofrece variantes de ancho:
- **Fuente estática**: con variantes "Condensed"/"Expanded" cargadas.
- **Fuente variable**: con eje `wdth`.

Si la fuente no tiene ancho variable, la propiedad no hace nada (el navegador no la deforma).

## font-width, el nombre moderno

La especificación renombró `font-stretch` a **`font-width`** (más claro). Ambos funcionan; `font-width` es la dirección del estándar. En fuentes variables, mueve el eje `wdth`, así que es preferible a [[03 font-variation-settings | `font-variation-settings: "wdth"`]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `font-stretch`/`font-width` para anchos, **no** `transform: scaleX` (que deforma).
> - Necesita una fuente con variantes de ancho o eje `wdth`.
> - El porcentaje da control fino con fuentes variables.
> - Útil para encajar texto en espacios estrechos sin reducir el tamaño.

## Errores comunes

> [!warning] Trampas
> - **`transform: scaleX`** en vez de `font-stretch`: deforma las letras.
> - **Esperar efecto** en una fuente sin variantes de ancho.
> - **Condensar en exceso**: por debajo de cierto ancho, la legibilidad cae.

## Notas relacionadas

- [[03 font-variation-settings]] — el eje `wdth` manual.
- [[04 font-weight]] — el eje de peso, su pariente.
- [[12 letter-spacing y word-spacing]] — ajustar espacio sin cambiar el ancho de las letras.
