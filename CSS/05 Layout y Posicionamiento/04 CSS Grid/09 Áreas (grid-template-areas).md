---
title: Áreas (grid-template-areas) — Maquetar con nombres visuales
aliases:
  - grid-template-areas
  - grid areas
tags:
  - css
  - api/propiedad
  - layout
propiedad: grid-template-areas
grupo: grid
valor_inicial: none
hereda: false
animable: false
draft: false
order: 9
---

# Áreas (grid-template-areas)

> [!definicion]
> `grid-template-areas` permite maquetar dibujando un **mapa ASCII** de la rejilla: das un nombre a cada zona y la "dibujas" con cadenas de texto. Luego cada item se asigna a su área por nombre. Es la forma más **legible** de definir layouts de página complejos.

```css
.layout {
  display: grid;
  grid-template-areas:
    "cabecera cabecera"
    "menu     contenido"
    "pie      pie";
  grid-template-columns: 200px 1fr;
}
.cabecera  { grid-area: cabecera; }
.menu      { grid-area: menu; }
.contenido { grid-area: contenido; }
.pie       { grid-area: pie; }
```

## El layout se "dibuja"

> [!tip] El mapa visual es autoexplicativo
> La gran virtud: el CSS **muestra** la forma del layout. Leyendo las cadenas se ve que la cabecera ocupa toda la fila superior, el menú está a la izquierda del contenido, y el pie abarca todo abajo. Cada **palabra** es una celda; repetir un nombre lo **extiende** por varias celdas:
> ```css
> grid-template-areas:
>   "cabecera cabecera cabecera"   /* la cabecera ocupa 3 columnas */
>   "menu     contenido contenido" /* el contenido, 2 columnas */
>   "pie      pie       pie";
> ```

## Reglas de la sintaxis

| Regla | Detalle |
|-------|---------|
| Cada cadena = una **fila** | Una línea de texto por fila de la rejilla |
| Cada palabra = una **celda** | Separadas por espacios |
| Nombre repetido = área extendida | Debe formar un **rectángulo** (no formas en L) |
| `.` (punto) = celda **vacía** | Una celda sin asignar |

> [!warning] Las áreas deben ser rectángulos
> Un nombre repetido tiene que formar un **rectángulo** contiguo. Una forma en "L" o discontinua es **inválida** y rompe toda la propiedad:
> ```css
> /* ❌ "menu" forma una L: inválido */
> "cabecera menu"
> "menu     contenido"
> ```

## Celdas vacías con el punto

Un `.` deja una celda **sin contenido**, útil para huecos en el layout:

```css
grid-template-areas:
  "logo  .     menu"
  "imagen texto texto";
```

## Reorganizar el layout en móvil

> [!tip] Cambiar el layout entero con una media query
> La mayor ventaja de las áreas: **redibujar** todo el layout en un breakpoint cambiando solo `grid-template-areas`, sin tocar los items ni el HTML:
> ```css
> @media (max-width: 600px) {
>   .layout {
>     grid-template-columns: 1fr;
>     grid-template-areas:    /* todo apilado en móvil */
>       "cabecera"
>       "menu"
>       "contenido"
>       "pie";
>   }
> }
> ```
> El layout pasa de dos columnas a una columna apilada con un solo cambio.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa áreas para layouts de página: el CSS "dibuja" la estructura, muy legible.
> - Asigna cada item con `grid-area: nombre`.
> - Reorganiza el layout en breakpoints reescribiendo solo `grid-template-areas`.
> - Recuerda: las áreas deben ser rectángulos; `.` para celdas vacías.

## Errores comunes

> [!warning] Trampas
> - **Área no rectangular** (forma en L): invalida la propiedad.
> - **Número de columnas inconsistente** entre las cadenas (cada fila debe tener las mismas).
> - **Nombre de área sin item asignado** (o viceversa): la celda queda vacía/el item flota.

## Notas relacionadas

- [[07 Ubicación por Líneas (grid-column, grid-row)]] — la alternativa por líneas.
- [[03 grid-template-columns y rows]] — definir el tamaño de las pistas.
- [[06 Layouts Responsivos/index]] — reorganizar layouts por breakpoint.
