---
title: Palabras Clave de Color — Nombres y palabras especiales
aliases:
  - color keywords
  - named colors
  - currentColor
tags:
  - css
  - api/concepto
  - fondos
draft: false
order: 1
---

# Palabras Clave de Color

> [!definicion]
> CSS reconoce **nombres** de color predefinidos (`red`, `tomato`, `rebeccapurple`) y varias **palabras clave especiales** con comportamiento dinámico (`transparent`, `currentColor`, `inherit`). Son la forma más legible de escribir un color, aunque limitada a una paleta fija.

```css
color: tomato;
background: transparent;
border-color: currentColor;
```

## Los colores con nombre

Hay **147 nombres** de color estandarizados (los "named colors"), desde los básicos hasta tonos específicos:

| Categoría | Ejemplos |
|-----------|----------|
| Básicos | `black`, `white`, `red`, `green`, `blue`, `yellow` |
| Grises | `gray`, `silver`, `dimgray`, `gainsboro`, `slategray` |
| Cálidos | `tomato`, `coral`, `salmon`, `gold`, `orange` |
| Fríos | `teal`, `navy`, `turquoise`, `steelblue`, `indigo` |
| Curiosos | `rebeccapurple`, `papayawhip`, `mistyrose`, `chartreuse` |

> [!info] rebeccapurple, el color con historia
> `rebeccapurple` (#663399) se añadió al estándar en memoria de Rebecca Meyer, hija del experto en CSS Eric Meyer, fallecida a los 6 años. Es el único color con nombre que conmemora a una persona.

## Las palabras clave especiales

Más útiles que los nombres concretos son las palabras dinámicas:

| Palabra | Significado |
|---------|-------------|
| `transparent` | Totalmente transparente (alfa 0). Equivale a `rgb(0 0 0 / 0)` |
| `currentColor` | El valor calculado de la propiedad `color` del elemento |
| `inherit` | Hereda el color del elemento padre |
| `initial` | El valor inicial de la propiedad |

### currentColor: el más valioso

> [!tip] currentColor sincroniza colores
> `currentColor` toma el valor actual de la propiedad `color`, lo que permite que **bordes, sombras, iconos SVG o fondos** sigan automáticamente al color de texto, sin repetirlo:
> ```css
> .boton {
>   color: #cba6f7;
>   border: 2px solid currentColor;   /* el borde iguala al texto */
> }
> .boton:hover { color: #f38ba8; }    /* el borde cambia solo */
> ```
> Cambiar `color` en `:hover` actualiza el borde "gratis". Es la base de iconos que heredan el color del texto que los acompaña.

### transparent en gradientes

`transparent` es clave en gradientes y transiciones de color, aunque cuidado: en algunos navegadores antiguos `transparent` equivalía a negro transparente, lo que producía bordes grisáceos al degradar hacia él. Hoy se interpola correctamente en el espacio de color adecuado.

## Ventajas y límites

| Ventaja | Límite |
|---------|--------|
| Muy legibles | Paleta fija (147 colores) |
| Rápidas para prototipar | Sin control fino ni alfa (salvo `transparent`) |
| `currentColor`/`inherit` son potentes | No sirven para una identidad de marca precisa |

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa nombres para prototipos rápidos; para producción, define colores de marca con hex/`oklch` en [[10 Variables CSS/index | variables]].
> - Aprovecha `currentColor` para que bordes, sombras e iconos sigan al texto.
> - `transparent` para huecos y degradados que se desvanecen.
> - No construyas una paleta de marca solo con nombres: son imprecisos y limitados.

## Errores comunes

> [!warning] Trampas
> - **Confundir tonos con nombre**: `gray` (#808080) ≠ `grey` (sí, ambos válidos y equivalentes), pero `darkgray` es **más claro** que `gray` (una rareza histórica).
> - **Esperar control fino**: para ajustar luz/saturación, usa [[04 HSL y HSLA | HSL]] u [[06 LAB y LCH | OKLCH]].
> - **Olvidar que `currentColor` depende de `color`**: si `color` cambia, todo lo que lo usa cambia.

## Notas relacionadas

- [[02 Hexadecimal]] — control preciso del color.
- [[02 Valores Especiales (inherit, initial, unset, revert)]] — `inherit`/`initial` en detalle.
- [[01 color]] — la propiedad `color` de la que depende `currentColor`.
