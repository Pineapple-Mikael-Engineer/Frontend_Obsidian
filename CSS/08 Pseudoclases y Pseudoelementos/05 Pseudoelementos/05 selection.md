---
title: selection — El texto seleccionado por el usuario
aliases:
  - "::selection"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: "::selection"
grupo: pseudoelemento
draft: false
order: 5
---

# ::selection

> [!definicion]
> `::selection` estiliza la parte del texto que el usuario ha **seleccionado** (resaltado con el ratón o el teclado). Permite cambiar el color de fondo y del texto resaltado, normalmente para que combine con la identidad visual del sitio en vez del azul por defecto del navegador.

```css
::selection { background: #cba6f7; color: white; }
```

## Personalizar el resaltado

> [!tip] El color de marca al seleccionar
> El uso típico: reemplazar el resaltado azul por defecto por el color de la marca:
> ```css
> ::selection {
>   background: #cba6f7;
>   color: #1e1e2e;
> }
> ```
> Un detalle pequeño pero pulido que da coherencia visual. Se puede aplicar globalmente (`::selection`) o a elementos concretos (`.cita::selection`).

## Propiedades muy limitadas

> [!warning] Solo unas pocas propiedades funcionan
> `::selection` acepta un conjunto **muy reducido** de propiedades:
> - `color`
> - `background-color`
> - `text-decoration`
> - `text-shadow`
> - `cursor` (en algunos navegadores)
>
> **No** acepta `font-size`, `padding`, `margin` ni nada que cambie el layout (cambiar el tamaño del texto al seleccionarlo causaría reflujos extraños). Es solo para el aspecto del resaltado.

## El contraste importa

> [!warning] No rompas la legibilidad al seleccionar
> Al personalizar `::selection`, asegúrate de que el texto siga siendo **legible** sobre el nuevo fondo. Cambiar solo el `background` sin ajustar el `color` puede dejar texto oscuro sobre fondo oscuro (ilegible). Define **ambos**:
> ```css
> /* ✅ fondo y color juntos, con buen contraste */
> ::selection { background: #cba6f7; color: #1e1e2e; }
> ```

## Por elemento

```css
/* Resaltado distinto en el código */
code::selection { background: #f9e2af; color: #1e1e2e; }

/* Resaltado en una sección oscura */
.hero::selection { background: white; color: #1e1e2e; }
```

## El prefijo -moz

> [!info] Firefox necesitó el prefijo
> Históricamente, Firefox requería `::-moz-selection`. Hoy `::selection` sin prefijo funciona en todos los navegadores modernos, pero en código antiguo puede verse el prefijo duplicado:
> ```css
> ::selection { background: #cba6f7; }
> ::-moz-selection { background: #cba6f7; }   /* respaldo histórico */
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Personaliza `::selection` con el color de marca para coherencia visual.
> - Define **fondo y color** juntos para mantener el contraste.
> - Recuerda que solo acepta color, fondo, `text-decoration` y `text-shadow`.
> - Aplica resaltados distintos a elementos concretos (código, secciones oscuras) si aporta.

## Errores comunes

> [!warning] Trampas
> - **Cambiar solo el fondo** y dejar el texto ilegible.
> - **Esperar cambiar el tamaño/fuente** al seleccionar: no se puede.
> - **Contraste insuficiente** del resaltado.

## Notas relacionadas

- [[01 Texto Resaltado (mark)]] — el resaltado permanente con `<mark>` (distinto).
- [[03 Sombra de Texto (text-shadow)]] — `text-shadow`, una de las pocas que acepta.
- [[index]] — los pseudoelementos en general.
