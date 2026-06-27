---
title: subgrid — Heredar la rejilla del padre
aliases:
  - subgrid
tags:
  - css
  - api/concepto
  - layout
draft: false
order: 6
---

# subgrid

> [!definicion]
> `subgrid` permite que un item grid que **también es contenedor grid** use las **pistas de su abuelo** en lugar de crear las suyas propias. Resuelve el problema clásico de **alinear contenido entre componentes hermanos** (que las cabeceras, cuerpos y pies de varias tarjetas queden en la misma línea).

```css
.tarjeta {
  display: grid;
  grid-template-rows: subgrid;   /* hereda las filas del grid padre */
  grid-row: span 3;
}
```

## El problema que resuelve

> [!info] Alinear el contenido interno de tarjetas hermanas
> Imagina una fila de tarjetas, cada una con título, texto y botón. Si los títulos tienen distinta cantidad de líneas, los botones quedan a **distinta altura** en cada tarjeta. Con grids independientes no se pueden alinear entre tarjetas. `subgrid` hace que las filas internas de cada tarjeta se **alineen con las del contenedor común**:
> ```css
> .galeria { display: grid; grid-template-columns: repeat(3, 1fr); }
> .tarjeta {
>   display: grid;
>   grid-row: span 3;              /* la tarjeta ocupa 3 filas del padre */
>   grid-template-rows: subgrid;   /* sus 3 filas = las del padre */
> }
> ```
> Así los títulos, cuerpos y botones de todas las tarjetas quedan **alineados en línea**, sin alturas fijas.

## Cómo funciona

`grid-template-columns: subgrid` o `grid-template-rows: subgrid` hace que ese eje del subgrid **adopte las líneas** de la rejilla padre que el item ocupa, en lugar de definir pistas nuevas. El item debe **abarcar** las pistas correspondientes del padre (con `grid-row: span N` o `grid-column: span N`).

## Sin subgrid: las alternativas imperfectas

> [!info] Antes de subgrid, los apaños
> Antes de `subgrid`, alinear contenido entre componentes hermanos requería apaños frágiles: alturas fijas (`min-height`), [[04 line-clamp | recortar texto]] a un número de líneas, o un solo grid gigante (perdiendo la encapsulación de cada tarjeta). `subgrid` resuelve esto de forma limpia, manteniendo cada tarjeta como un componente independiente.

## Soporte

> [!info] Soporte moderno, ya generalizado
> `subgrid` tardó en llegar pero hoy tiene **buen soporte** en todos los navegadores principales. Para proyectos que deban soportar navegadores antiguos, conviene una alternativa de respaldo (alturas o line-clamp); en la web actual, es seguro usarlo.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `subgrid` para alinear el contenido interno de componentes hermanos (tarjetas, columnas).
> - El subgrid debe abarcar las pistas del padre con `grid-row/column: span N`.
> - Mantiene cada componente encapsulado, a diferencia de un grid único gigante.
> - Da un respaldo (line-clamp/min-height) solo si soportas navegadores muy antiguos.

## Errores comunes

> [!warning] Trampas
> - **No abarcar las pistas del padre**: el subgrid necesita `span` o coordenadas que cubran las filas/columnas.
> - **Esperar que cree pistas nuevas**: `subgrid` **hereda** las del padre, no define propias.
> - **Confundir el eje**: puedes hacer subgrid solo de filas, solo de columnas, o ambos.

## Notas relacionadas

- [[01 Contenedor Grid]] — el grid padre y el subgrid.
- [[08 Span]] — abarcar varias pistas, requisito del subgrid.
- [[04 line-clamp]] — el apaño que subgrid reemplaza.
