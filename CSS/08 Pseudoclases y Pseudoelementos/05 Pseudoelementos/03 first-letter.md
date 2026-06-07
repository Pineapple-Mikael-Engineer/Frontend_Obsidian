---
title: first-letter — La primera letra (capitulares)
aliases:
  - "::first-letter"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: "::first-letter"
grupo: pseudoelemento
draft: false
---

# ::first-letter

> [!definicion]
> `::first-letter` selecciona la **primera letra** de un bloque de texto, para estilarla de forma distinta. Su uso clásico es la **capitular** (drop cap): la letra inicial grande y decorada de un párrafo, como en los libros antiguos.

```css
p::first-letter { font-size: 3em; font-weight: bold; }
```

## La capitular clásica

> [!tip] La letra inicial grande de un artículo
> El efecto editorial por excelencia: la primera letra grande que "cae" sobre las primeras líneas:
> ```css
> .articulo > p:first-of-type::first-letter {
>   font-size: 3.5em;
>   font-weight: bold;
>   float: left;              /* el texto fluye alrededor */
>   line-height: 0.8;
>   margin: 0.1em 0.1em 0 0;
>   color: #cba6f7;
> }
> ```
> El `float: left` hace que las siguientes líneas se ajusten alrededor de la letra grande, el aspecto de capitular de revista.

## Qué cuenta como "primera letra"

> [!info] Incluye la puntuación inicial
> `::first-letter` incluye la **puntuación** que precede o sigue inmediatamente a la letra (comillas, paréntesis). Si un párrafo empieza con `"Hola`, la capitular abarca la comilla y la H. El navegador es inteligente con esto, pero conviene tenerlo en cuenta para el espaciado.

## Propiedades aplicables limitadas

> [!warning] Solo ciertas propiedades funcionan
> `::first-letter` solo acepta un **subconjunto** de propiedades: las de fuente (`font-*`), color, fondo, márgenes, padding, borde, `float`, `text-transform`, `line-height`, `vertical-align`. Propiedades de layout más complejas no aplican. Es suficiente para capitulares, pero no esperes control total.

## Aplicar solo al primer párrafo

> [!tip] Combinar con :first-of-type
> Normalmente la capitular solo va en el **primer** párrafo, no en todos. Combina `::first-letter` con [[02 first-of-type y last-of-type | `:first-of-type`]]:
> ```css
> article p:first-of-type::first-letter { font-size: 3em; float: left; }
> ```
> Así solo el primer párrafo del artículo lleva la capitular, aunque haya un `<h2>` antes.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `::first-letter` + `float: left` para capitulares de revista.
> - Aplícala solo al primer párrafo con `:first-of-type`.
> - Ajusta `line-height` y `margin` para que la letra encaje con las líneas.
> - Recuerda que solo acepta ciertas propiedades (fuente, color, float…).

## Errores comunes

> [!warning] Trampas
> - **Capitular en todos los párrafos** por no usar `:first-of-type`.
> - **Esperar que funcione cualquier propiedad**: solo un subconjunto aplica.
> - **Espaciado raro** por la puntuación inicial incluida en la "primera letra".

## Notas relacionadas

- [[04 first-line]] — la primera línea (paralelo).
- [[02 first-of-type y last-of-type]] — aplicar solo al primer párrafo.
- [[01 float]] — el `float` que hace fluir el texto alrededor.
