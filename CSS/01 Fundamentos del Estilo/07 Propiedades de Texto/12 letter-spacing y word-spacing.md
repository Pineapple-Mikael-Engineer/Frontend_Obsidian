---
title: letter-spacing y word-spacing — Espaciado entre letras y palabras
aliases:
  - letter-spacing
  - word-spacing
  - tracking
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: letter-spacing
grupo: tipografia
valor_inicial: normal
hereda: true
animable: true
draft: false
order: 12
---

# letter-spacing y word-spacing

> [!definicion]
> `letter-spacing` ajusta el espacio **entre letras** (lo que en tipografía se llama *tracking*); `word-spacing`, el espacio **entre palabras**. Permiten afinar la densidad del texto, abriendo o cerrando el espaciado para mejorar la legibilidad o lograr un efecto.

```css
.titulo { letter-spacing: 0.05em; }
.amplio { word-spacing: 0.3em; }
```

## Valores

| Valor | Efecto |
|-------|--------|
| `normal` | El espaciado natural de la fuente (por defecto) |
| Longitud positiva | **Aumenta** el espacio (`0.1em`, `2px`) |
| Longitud negativa | **Reduce** el espacio (`-0.02em`) |

## Usar em, no px

> [!tip] letter-spacing en em escala con la fuente
> Como el espaciado ideal es **proporcional** al tamaño de letra, conviene expresarlo en `em` (relativo al `font-size`) en vez de `px`. Así, un título grande y uno pequeño con `letter-spacing: 0.05em` mantienen una densidad coherente:
> ```css
> h1, h2 { letter-spacing: 0.03em; }   /* escala con cada tamaño */
> ```

## Los casos de uso reales

| Caso | Ajuste |
|------|--------|
| Mayúsculas / versalitas | `letter-spacing` ligeramente **positivo** (las mayúsculas necesitan aire) |
| Títulos grandes | A veces `letter-spacing` **negativo** (las letras grandes parecen separadas) |
| Logotipos, etiquetas | Tracking amplio para efecto |
| Texto corrido | **No tocar**: la fuente ya está optimizada |

## No abusar en texto corrido

> [!warning] El espaciado excesivo daña la legibilidad
> Las fuentes vienen con un espaciado entre letras **cuidadosamente diseñado**. Alterarlo en texto corrido casi siempre **empeora** la legibilidad: demasiado tracking rompe la percepción de las palabras como unidades; muy poco, las amontona. Reserva `letter-spacing` para mayúsculas, títulos y efectos puntuales, no para párrafos. Las WCAG incluso fijan un mínimo de espaciado que el texto debe **tolerar** (criterio de espaciado), así que no conviene fijar valores que lo impidan.

## word-spacing, de uso más raro

`word-spacing` se usa menos: el espacio entre palabras de la fuente suele ser adecuado. Tiene nichos (abrir un poco el texto justificado, efectos de diseño), pero rara vez es necesario en uso normal.

## Buenas prácticas

> [!tip] Recomendaciones
> - Expresa `letter-spacing` en `em` para que escale con el tamaño.
> - Aumenta ligeramente el tracking en mayúsculas y versalitas.
> - No toques el espaciado de texto corrido: la fuente ya lo optimiza.
> - Úsalo para títulos, etiquetas y efectos, no para cuerpo.

## Errores comunes

> [!warning] Trampas
> - **Tracking excesivo en párrafos**: rompe la legibilidad.
> - **`px` en vez de `em`**: el espaciado no escala con el tamaño.
> - **Valores que impiden el espaciado mínimo** de las WCAG.

## Notas relacionadas

- [[10 text-transform]] — mayúsculas, que se benefician de más tracking.
- [[07 line-height]] — el espaciado vertical, su contraparte.
- [[06 font-variant]] — versalitas, otro caso de tracking aumentado.
