---
title: text-align — Alineación horizontal del texto
aliases:
  - text-align
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: text-align
grupo: tipografia
valor_inicial: start
hereda: true
animable: false
draft: false
---

# text-align

> [!definicion]
> `text-align` alinea el **contenido en línea** (texto, imágenes inline) dentro de su bloque: a la izquierda, derecha, centrado o justificado. Solo afecta a contenido en línea; para centrar **bloques** se usan otras técnicas (`margin: auto`, Flexbox).

```css
h1      { text-align: center; }
.precio { text-align: right; }
article { text-align: justify; }
```

## Valores

| Valor | Efecto |
|-------|--------|
| `start` | Al inicio según la dirección del texto (izq. en LTR) — valor inicial |
| `end` | Al final (der. en LTR) |
| `left` | Siempre a la izquierda |
| `right` | Siempre a la derecha |
| `center` | Centrado |
| `justify` | Estira el texto para llenar la línea (bordes rectos a ambos lados) |

## start/end vs. left/right

> [!tip] Prefiere start/end para internacionalización
> `start` y `end` se adaptan a la **dirección** del texto: en idiomas RTL (árabe, hebreo), `start` es la derecha. `left`/`right` son fijos. Para sitios que puedan ser multilingües, `start`/`end` (y las propiedades lógicas en general) son más robustos que `left`/`right`:
> ```css
> .menu { text-align: start; }   /* izq. en español, der. en árabe */
> ```

## justify y sus problemas

> [!warning] El justificado puede dañar la legibilidad
> `justify` estira los espacios para que ambos bordes queden rectos, pero sin control fino crea **"ríos"** de espacio blanco irregulares entre palabras, sobre todo en columnas estrechas, dañando la lectura. Para usarlo bien conviene activar el guionado con [[03 hyphens | `hyphens: auto`]], que parte palabras y reparte mejor el espacio:
> ```css
> p { text-align: justify; hyphens: auto; }
> ```
> En la web, el justificado se usa con cautela; el alineado a la izquierda (`start`) suele leerse mejor.

## No centra bloques

> [!warning] text-align: center no centra cajas
> `text-align: center` centra el **contenido en línea** dentro del elemento, no el elemento en sí. Para centrar un bloque (un `<div>`, una imagen de bloque) horizontalmente:
> - `margin-inline: auto` (con un `width`/`max-width` definido).
> - O un contenedor [[01 Contenedor Flex (display flex) | Flexbox]] con `justify-content: center`.
>
> `text-align: center` en el **padre** sí centra hijos en línea o `inline-block`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Texto corrido: `start` (izquierda en LTR) suele ser lo más legible.
> - `center` para títulos cortos y elementos puntuales, no para párrafos largos.
> - `justify` solo con `hyphens: auto` y en líneas no demasiado estrechas.
> - Usa `start`/`end` en vez de `left`/`right` para soportar RTL.

## Errores comunes

> [!warning] Trampas
> - **Esperar que centre un bloque**: centra contenido en línea, no la caja.
> - **`justify` sin guionado** en columnas estrechas: ríos de espacio.
> - **Párrafos largos centrados**: difíciles de leer (el ojo pierde el inicio de línea).

## Notas relacionadas

- [[05 Margin]] — `margin: auto` para centrar bloques.
- [[03 hyphens]] — guionado para justificar bien.
- [[02 direction]] — la dirección que `start`/`end` respetan.
