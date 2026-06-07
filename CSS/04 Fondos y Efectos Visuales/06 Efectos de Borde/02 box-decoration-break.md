---
title: box-decoration-break — Decoración en elementos partidos
aliases:
  - box-decoration-break
tags:
  - css
  - api/propiedad
  - fondos
propiedad: box-decoration-break
grupo: box-model
valor_inicial: slice
hereda: false
animable: false
draft: false
---

# box-decoration-break

> [!definicion]
> `box-decoration-break` controla cómo se aplican los **bordes, fondos, padding y sombras** cuando un elemento en línea se **parte** en varias líneas (o una caja se divide entre columnas/páginas). Decide si la decoración se trata como una caja continua (`slice`) o se aplica a cada fragmento (`clone`).

```css
.resaltado {
  box-decoration-break: clone;
  -webkit-box-decoration-break: clone;
}
```

## El problema: texto resaltado que rompe en dos líneas

> [!info] Bordes y fondos que se cortan
> Cuando un `<span>` con fondo, padding y `border-radius` ocupa **dos líneas**, ¿qué pasa con su decoración? Por defecto (`slice`), el navegador trata el elemento como una caja continua que **se corta** por donde rompe la línea: el `border-radius` solo aparece en los extremos absolutos, el padding falta en los cortes, y el resultado se ve roto. `clone` arregla esto.

## Los dos valores

| Valor | Comportamiento |
|-------|----------------|
| `slice` | La decoración es de una caja continua, **cortada** en las roturas (por defecto) |
| `clone` | Cada fragmento recibe la decoración **completa** (su propio borde, padding, radio) |

## clone: cada línea con su decoración

> [!tip] clone para resaltados multilínea bonitos
> El uso estrella: un texto resaltado (con fondo, padding y esquinas redondeadas) que se ve **bien en cada línea** aunque rompa, como un rotulador que envuelve cada fragmento:
> ```css
> .marcador {
>   background: #cba6f7;
>   padding: 0.2em 0.4em;
>   border-radius: 0.3em;
>   box-decoration-break: clone;          /* cada línea, su propia cápsula */
>   -webkit-box-decoration-break: clone;  /* prefijo aún necesario */
> }
> ```
> Con `clone`, cada trozo del texto tiene su padding y sus esquinas redondeadas; con `slice` (por defecto), se vería cortado y feo.

## Otros contextos

`box-decoration-break` también aplica cuando una caja se divide entre **columnas** ([[01 column-count y column-width | multicolumna]]) o **páginas** (al imprimir): `clone` hace que cada fragmento tenga su borde/fondo completos.

## El prefijo

> [!warning] Aún requiere -webkit- en varios navegadores
> Como [[02 mask | `mask`]] y [[03 text-stroke | `text-stroke`]], conviene incluir el prefijo `-webkit-box-decoration-break` junto a la propiedad sin prefijo para máxima compatibilidad.

## Buenas prácticas

> [!tip] Recomendaciones
> - `clone` para texto resaltado con fondo/padding/radio que pueda romper en varias líneas.
> - Incluye el prefijo `-webkit-` como respaldo.
> - Recuerda que también afecta a cajas partidas entre columnas o páginas.
> - Por defecto (`slice`) está bien para decoración simple que no se note cortada.

## Errores comunes

> [!warning] Trampas
> - **Resaltado multilínea sin `clone`**: se ve cortado en las roturas de línea.
> - **Olvidar el prefijo `-webkit-`**: no funciona en algunos navegadores.

## Notas relacionadas

- [[04 Shorthand border]] · [[01 background-color]] — la decoración que se parte.
- [[12 Texto Resaltado (mark)]] — el resaltado de texto desde HTML.
- [[01 column-count y column-width]] — el contexto de columnas.
