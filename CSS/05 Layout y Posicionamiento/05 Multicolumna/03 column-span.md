---
title: column-span — Abarcar todas las columnas
aliases:
  - column-span
tags:
  - css
  - api/propiedad
  - layout
propiedad: column-span
grupo: layout
valor_inicial: none
hereda: false
animable: false
draft: false
order: 3
---

# column-span

> [!definicion]
> `column-span` hace que un elemento dentro de un layout multicolumna **abarque todas las columnas**, rompiendo el flujo de columnas para ocupar todo el ancho. Es el típico **titular** que cruza un artículo de periódico de lado a lado.

```css
.articulo h2 {
  column-span: all;   /* el título cruza todas las columnas */
}
```

## Valores

| Valor | Efecto |
|-------|--------|
| `none` | El elemento fluye en su columna (por defecto) |
| `all` | El elemento abarca **todas** las columnas |

(No existe un valor para abarcar un número parcial de columnas: es todo o nada.)

## El titular que cruza el artículo

> [!tip] El uso clásico: encabezados a todo lo ancho
> El caso de uso por excelencia: en un texto en columnas, un titular o una imagen destacada que cruza **todo el ancho**, partiendo el flujo en dos bloques de columnas:
> ```css
> .revista { columns: 16rem; column-gap: 2rem; }
> .revista h2, .revista .destacado {
>   column-span: all;   /* cruzan todas las columnas */
> }
> ```
> El texto fluye en columnas hasta el titular, este ocupa todo el ancho, y el texto sigue en columnas debajo. Es la maqueta de revista típica.

## Reinicia el equilibrio de columnas

> [!info] El contenido se reequilibra alrededor del span
> Cuando un elemento tiene `column-span: all`, el contenido de columnas **antes** y **después** de él se trata como bloques separados, cada uno equilibrando sus columnas de forma independiente. Por eso un titular a todo lo ancho "parte" el artículo en secciones de columnas.

## Soporte

`column-span: all` tiene buen soporte en navegadores modernos. Históricamente Firefox tardó en implementarlo, pero hoy es seguro.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `column-span: all` para titulares e imágenes destacadas que deban cruzar las columnas.
> - Es todo o nada: no se puede abarcar un número parcial de columnas.
> - Combínalo con `break-inside: avoid` en elementos que no deban partirse.
> - Útil para maquetas tipo revista/periódico.

## Errores comunes

> [!warning] Trampas
> - **Esperar abarcar columnas parciales**: solo `all` o `none`.
> - **Usarlo fuera de un contexto multicolumna**: no hace nada sin `column-count`/`column-width` en el padre.

## Notas relacionadas

- [[01 column-count y column-width]] — el contexto multicolumna.
- [[04 break-inside]] — evitar cortes en otros elementos.
- [[09 Áreas (grid-template-areas)]] — abarcar columnas en Grid (otro contexto).
