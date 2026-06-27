---
title: width y height — Tamaño base de la caja
aliases:
  - width
  - height
tags:
  - css
  - api/propiedad
  - layout
propiedad: width
grupo: box-model
valor_inicial: auto
hereda: false
animable: true
draft: false
order: 1
---

# width y height

> [!definicion]
> `width` y `height` fijan el **ancho** y el **alto** del área de contenido de una caja (o de toda la caja con `border-box`). Su valor por defecto, `auto`, deja que el navegador calcule el tamaño según el contenido y el contexto de layout.

```css
.caja { width: 300px; height: 200px; }
img   { width: 100%; height: auto; }   /* responsiva, mantiene proporción */
```

## El valor auto, el más importante

> [!info] auto se comporta distinto en width y en height
> `auto` es el valor por defecto, y su significado depende del eje y del tipo de elemento:
> - **`width: auto`** en un bloque: ocupa **todo el ancho disponible** del contenedor (menos sus márgenes). Por eso un `<div>` sin `width` llena la línea.
> - **`height: auto`**: toma la **altura de su contenido** (crece con lo que contiene). Por eso no se suele fijar `height`: dejar que el contenido la determine evita cortes.

## width: 100% vs. width: auto

Parecen iguales pero no lo son:

| | `width: auto` | `width: 100%` |
|--|---------------|----------------|
| Tamaño | El disponible menos los márgenes | El 100% del contenedor (sin restar márgenes) |
| Con margen | Se ajusta | Puede **desbordar** (100% + margen > contenedor) |

> [!warning] 100% + padding/margin puede desbordar
> Con `box-sizing: content-box`, un `width: 100%` más padding o margen suma **más** que el contenedor, causando scroll horizontal. `width: auto` (o `box-sizing: border-box`) evita el problema. Por eso muchas veces no hace falta declarar `width: 100%` en bloques: `auto` ya llena el ancho de forma segura.

## El peligro de fijar height

> [!warning] No fijes height para contenido de texto
> Fijar una `height` rígida a un contenedor de texto es arriesgado: si el contenido crece (más texto, otra fuente, traducción a un idioma más largo), **se desborda** o se corta. Prefiere:
> - `min-height` para garantizar una altura mínima dejando crecer.
> - Dejar `height: auto` (el contenido manda).
>
> ```css
> .tarjeta { min-height: 10rem; }   /* mínimo, pero crece si hace falta */
> ```

## Imágenes responsivas

El patrón estándar para imágenes fluidas que no se deforman:

```css
img { max-width: 100%; height: auto; }
```

`max-width: 100%` evita que la imagen desborde su contenedor; `height: auto` mantiene la proporción original. Combinar con [[01 Imagen (img) | dimensiones en el HTML]] reserva el espacio y evita saltos.

## Buenas prácticas

> [!tip] Recomendaciones
> - Deja `height: auto` (el contenido define la altura); usa `min-height` si necesitas un mínimo.
> - Para bloques, `auto` suele bastar; reserva `width` para tamaños concretos.
> - Combina `width` fluido con `max-width` en `rem` para contenedores responsivos.
> - `img { max-width: 100%; height: auto; }` para imágenes que no desbordan ni se deforman.

## Errores comunes

> [!warning] Trampas
> - **`height` fija** en texto: se desborda o corta al crecer el contenido.
> - **`width: 100%` + padding** con `content-box`: desborda (usa `border-box` o `auto`).
> - **Fijar `width` y `height`** a una imagen sin respetar su proporción: se deforma.

## Notas relacionadas

- [[02 min y max (width, height)]] — acotar el tamaño.
- [[06 box-sizing]] — qué incluye `width`.
- [[03 aspect-ratio]] — fijar proporción en vez de alto.
