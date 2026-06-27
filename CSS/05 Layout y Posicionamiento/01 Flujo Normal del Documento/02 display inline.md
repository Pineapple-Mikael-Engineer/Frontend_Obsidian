---
title: display inline — Elemento en línea
aliases:
  - display inline
tags:
  - css
  - api/propiedad
  - layout
propiedad: display
grupo: layout
valor_inicial: inline
hereda: false
animable: false
draft: false
order: 2
---

# display inline

> [!definicion]
> Un elemento con `display: inline` fluye **dentro del texto**, uno tras otro en la misma línea, ocupando solo el ancho de su contenido. **Ignora** `width` y `height`, y solo respeta parcialmente los márgenes y paddings. Es el comportamiento de `<span>`, `<a>`, `<strong>`, `<em>`.

```css
.resaltar { display: inline; }   /* (ya es el valor por defecto de un span) */
```

## Características

| Aspecto | Comportamiento |
|---------|----------------|
| Salto de línea | No: fluye con el texto |
| Ancho | Solo el de su contenido |
| `width`/`height` | **Se ignoran** |
| `margin`/`padding` vertical | El padding se ve, pero **no desplaza** las líneas vecinas |
| `margin`/`padding` horizontal | Se respetan |

## Las dos grandes limitaciones

> [!warning] inline ignora width/height y el margin vertical
> Dos cosas sorprenden de los elementos en línea:
> 1. **`width`/`height` no hacen nada**: un `<span>` con `width: 200px` sigue ocupando lo que su texto necesita.
> 2. **El padding/margin vertical no empuja** a las líneas de arriba y abajo: un `padding-top` en un `<span>` puede solaparse con la línea superior en lugar de separarla.
>
> Si necesitas dimensiones o espaciado vertical real en un elemento en línea, cámbialo a [[03 display inline-block | `inline-block`]] o `block`.

## Elementos inline por defecto

`<span>`, `<a>`, `<strong>`, `<em>`, `<b>`, `<i>`, `<code>`, `<abbr>`, `<img>` (aunque `<img>` acepta dimensiones, es un caso especial "inline replaced"), `<label>`, `<button>` (inline-block en realidad).

## Cuándo usar inline

`inline` es para marcar **partes del texto** sin romper su flujo: resaltar una palabra, enlazar dentro de una frase, aplicar color a un fragmento. Si el elemento debe comportarse como una "caja" (con tamaño, en fila con otros), no es `inline`.

## El espacio en blanco entre inline

> [!info] Los huecos fantasma entre elementos inline
> Entre dos elementos en línea (o inline-block) separados por un salto de línea o espacio en el HTML, aparece un **espacio en blanco** (el del código fuente). Esto causa "huecos" inesperados al alinear inline-blocks. Con [[01 Contenedor Flex (display flex) | Flexbox]] este problema desaparece (no hay espacio entre items flex), razón más para preferirlo al maquetar.

## Buenas prácticas

> [!tip] Recomendaciones
> - `inline` para marcar fragmentos de texto (resaltados, enlaces dentro de una frase).
> - Si necesitas `width`/`height` o espaciado vertical, usa `inline-block` o `block`.
> - Para colocar elementos en fila con control, usa Flexbox (sin huecos fantasma).
> - No fuerces `inline` en contenedores que deben tener dimensiones.

## Errores comunes

> [!warning] Trampas
> - **Poner `width`/`height`** a un inline: se ignoran.
> - **Esperar que el padding vertical separe** las líneas: no lo hace.
> - **Huecos fantasma** entre inline-blocks por el espacio del HTML.

## Notas relacionadas

- [[01 display block]] — el comportamiento opuesto.
- [[03 display inline-block]] — en línea pero con dimensiones.
- [[01 Contenedor Flex (display flex)]] — alinear sin los problemas de inline.
