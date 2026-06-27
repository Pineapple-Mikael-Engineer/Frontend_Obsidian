---
title: vertical-align — Alineación vertical de inline y celdas
aliases:
  - vertical-align
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: vertical-align
grupo: tipografia
valor_inicial: baseline
hereda: false
animable: false
draft: false
order: 1
---

# vertical-align

> [!definicion]
> `vertical-align` alinea verticalmente elementos **en línea** (`inline`, `inline-block`) y **celdas de tabla** respecto a la línea de texto que los rodea. Es una de las propiedades peor entendidas de CSS, porque **no** sirve para centrar verticalmente bloques (para eso está Flexbox/Grid).

```css
sub { vertical-align: sub; }
.icono { vertical-align: middle; }
td { vertical-align: top; }
```

## Dónde funciona (y dónde no)

> [!warning] No centra bloques verticalmente
> El malentendido más común: `vertical-align: middle` **no** centra un `<div>` dentro de otro. Solo funciona en:
> - Elementos **en línea** (`inline`, `inline-block`): los alinea respecto a la línea de texto.
> - **Celdas de tabla** (`<td>`, `display: table-cell`): alinea el contenido en la celda.
>
> Para centrar un bloque verticalmente, usa [[05 Eje Transversal (align-items, align-self) | Flexbox `align-items: center`]] o Grid. `vertical-align` es para texto e inline, no para layout de bloques.

## Valores

| Valor | Alinea respecto a |
|-------|-------------------|
| `baseline` | La línea base del texto (por defecto) |
| `middle` | El medio de la x-height de la línea |
| `top` / `bottom` | La parte superior/inferior de la línea |
| `text-top` / `text-bottom` | La parte superior/inferior del **texto** |
| `sub` / `super` | Posición de subíndice/superíndice |
| Longitud / % | Desplaza desde la línea base (`0.2em`, `-30%`) |

## El caso de los iconos junto al texto

> [!tip] Alinear un icono SVG con el texto
> Un uso muy común: un icono inline (SVG, imagen) junto a texto suele quedar desalineado por la línea base. `vertical-align` lo corrige:
> ```css
> .icono { vertical-align: middle; }      /* o -0.125em para ajuste fino */
> ```
> A menudo `middle` no basta y se ajusta con una longitud (`vertical-align: -0.15em`) hasta que el icono cuadre con el texto.

## El misterio del hueco bajo las imágenes

> [!info] El espacio fantasma bajo una <img>
> Un problema clásico: una imagen dentro de un contenedor deja un **hueco** de unos píxeles debajo. La causa es `vertical-align: baseline` (por defecto): la imagen se alinea con la línea base del texto, dejando sitio para los "descendentes" (la cola de la "p", la "g"). Soluciones:
> ```css
> img { vertical-align: middle; }   /* o bottom, o display: block */
> ```
> Es uno de los bugs de maquetación más confusos para principiantes.

## En celdas de tabla

En celdas (`<td>`), `vertical-align` alinea el contenido arriba, en medio o abajo de la celda, lo que **sí** funciona como "centrado vertical" en ese contexto:

```css
td { vertical-align: top; }   /* contenido alineado arriba en cada celda */
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `vertical-align` para alinear iconos/inline con el texto y contenido en celdas.
> - Para centrar **bloques** verticalmente, usa Flexbox/Grid, no `vertical-align`.
> - Ajusta iconos con una longitud (`-0.15em`) si `middle` no cuadra.
> - El hueco bajo imágenes se quita con `vertical-align: middle/bottom` o `display: block`.

## Errores comunes

> [!warning] Trampas
> - **Esperar que centre un bloque**: solo afecta a inline y celdas.
> - **El hueco fantasma** bajo imágenes por la línea base.
> - **`middle` no cuadra**: necesita ajuste fino con longitud.

## Notas relacionadas

- [[05 Eje Transversal (align-items, align-self)]] — centrar bloques verticalmente con Flexbox.
- [[03 display inline-block]] — el `display` donde `vertical-align` aplica.
- [[07 line-height]] — el alto de línea que define la referencia.
