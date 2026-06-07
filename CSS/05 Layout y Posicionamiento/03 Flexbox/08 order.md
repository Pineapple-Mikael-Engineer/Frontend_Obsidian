---
title: order — Reordenar items visualmente
aliases:
  - order
tags:
  - css
  - api/propiedad
  - layout
propiedad: order
grupo: flexbox
valor_inicial: "0"
hereda: false
animable: false
draft: false
---

# order

> [!definicion]
> `order` cambia el **orden visual** de un item flex (o grid) sin tocar el HTML. Los items se ordenan por su valor de `order` (menor primero); con el mismo valor, por su orden en el DOM. El valor por defecto es `0`.

```css
.destacado { order: -1; }   /* aparece antes que los demás */
```

## Cómo funciona

| `order` | Posición |
|---------|----------|
| `-1` | Antes de los items con order 0 |
| `0` | Por defecto (orden del DOM) |
| `1` | Después de los items con order 0 |

Los items se colocan de menor a mayor `order`; los empates se resuelven por el orden del HTML.

## Caso de uso: reordenar por breakpoint

> [!tip] Cambiar el orden en móvil sin tocar el HTML
> El uso más legítimo: mostrar elementos en distinto orden según el tamaño de pantalla. Por ejemplo, en móvil poner una imagen antes del texto, y en escritorio al revés, sin duplicar el marcado:
> ```css
> .articulo { display: flex; flex-direction: column; }
> @media (min-width: 768px) {
>   .articulo { flex-direction: row; }
>   .articulo .imagen { order: 1; }   /* la imagen a la derecha en escritorio */
> }
> ```

## El gran problema: accesibilidad

> [!warning] order rompe el orden de tabulación
> `order` cambia el orden **visual** pero **no** el del DOM. Esto significa que:
> - El orden de **tabulación** (teclado) sigue el del HTML, no el visual.
> - El orden de **lectura** de los lectores de pantalla sigue el del HTML.
>
> Resultado: un usuario de teclado puede tabular en un orden que **no coincide** con lo que ve, una desconexión desorientadora que viola las WCAG (orden de foco significativo). **Regla**: usa `order` solo cuando el orden visual no sea crítico para la comprensión, y nunca para reordenar contenido cuyo orden importe. Si el orden importa de verdad, cámbialo en el HTML.

## order vs. cambiar el HTML

| Quiero… | Mejor opción |
|---------|--------------|
| Reordenar visualmente sin que el orden importe | `order` (con cuidado) |
| Que el orden de lectura/foco coincida con lo visual | Cambiar el **HTML** |
| Reordenar solo en un breakpoint | `order` en media query (si no rompe el foco) |

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `order` solo para ajustes visuales donde el orden de foco/lectura no sea crítico.
> - **Nunca** reordenes con `order` contenido cuyo orden importe para entenderlo.
> - Prueba la navegación por teclado tras usar `order`: ¿el foco sigue un orden lógico?
> - Si el orden de verdad cambia, ajusta el HTML, no solo el CSS.

## Errores comunes

> [!warning] Trampas
> - **Desconexión visual/foco**: el teclado tabula en un orden distinto al que se ve.
> - **Abusar de `order`** para maquetar en vez de estructurar bien el HTML.
> - **Reordenar contenido crítico** rompiendo la lógica de lectura.

## Notas relacionadas

- [[02 Dirección (flex-direction)]] — `row-reverse`/`column-reverse`, con el mismo problema de accesibilidad.
- [[01 tabindex]] — el orden de foco que `order` no cambia.
- [[07 Ubicación por Líneas (grid-column, grid-row)]] — reubicar en Grid (mismo cuidado).
