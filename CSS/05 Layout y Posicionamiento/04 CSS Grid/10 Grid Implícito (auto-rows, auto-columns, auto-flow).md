---
title: Grid Implícito — auto-rows, auto-columns, auto-flow
aliases:
  - implicit grid
  - grid-auto-rows
  - grid-auto-flow
tags:
  - css
  - api/propiedad
  - layout
propiedad: grid-auto-rows
grupo: grid
valor_inicial: auto
hereda: false
animable: false
draft: false
---

# Grid Implícito (auto-rows, auto-columns, auto-flow)

> [!definicion]
> Cuando hay **más items** de los que caben en la rejilla **explícita**, Grid crea pistas **implícitas** automáticamente. `grid-auto-rows`/`grid-auto-columns` definen el **tamaño** de esas pistas creadas, y `grid-auto-flow` controla **cómo se colocan** los items que fluyen automáticamente.

```css
.galeria {
  display: grid;
  grid-template-columns: repeat(3, 1fr);   /* explícito: 3 columnas */
  grid-auto-rows: 200px;                    /* implícito: filas de 200px */
}
```

## Explícito vs. implícito

> [!info] Lo que defines y lo que Grid añade
> - **Explícito**: las pistas que declaras con `grid-template-columns`/`rows`.
> - **Implícito**: las pistas que Grid **crea solo** para acomodar items que no caben en lo explícito.
>
> Lo habitual: defines las **columnas** (explícito) y dejas que las **filas** se creen solas según el número de items (implícito). `grid-auto-rows` da el alto a esas filas automáticas:
> ```css
> grid-template-columns: repeat(3, 1fr);   /* 3 columnas fijas */
> grid-auto-rows: minmax(150px, auto);     /* cada fila nueva: mín 150px, crece con el contenido */
> ```

## grid-auto-flow: la dirección del flujo

`grid-auto-flow` decide en qué orden y dirección se colocan los items automáticos:

| Valor | Comportamiento |
|-------|----------------|
| `row` | Llena fila por fila (por defecto) |
| `column` | Llena columna por columna |
| `dense` | Rellena los **huecos** que dejan items grandes (puede reordenar) |

## dense: rellenar huecos

> [!tip] dense compacta la rejilla
> Cuando hay items que ocupan varias celdas ([[08 Span | span]]), quedan **huecos** que el flujo normal no rellena. `grid-auto-flow: dense` hace que items posteriores **retrocedan** a llenar esos huecos, compactando la rejilla (estilo "mosaico de Pinterest"):
> ```css
> .galeria {
>   display: grid;
>   grid-template-columns: repeat(4, 1fr);
>   grid-auto-flow: dense;   /* rellena los huecos de los items grandes */
> }
> ```
> **Cuidado**: `dense` puede cambiar el **orden visual** respecto al DOM (un item posterior salta a un hueco anterior), con el mismo problema de accesibilidad que [[08 order | `order`]]. Úsalo cuando el orden no sea crítico.

## column flow: galerías horizontales

`grid-auto-flow: column` (con `grid-auto-columns`) crea rejillas que crecen **horizontalmente**, útil para carruseles o líneas de tiempo con scroll horizontal:

```css
.carrusel {
  display: grid;
  grid-auto-flow: column;
  grid-auto-columns: 250px;
  overflow-x: auto;
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Define columnas explícitas; usa `grid-auto-rows` para el alto de las filas automáticas.
> - `grid-auto-flow: dense` para compactar rejillas con items de distinto tamaño (si el orden no importa).
> - `grid-auto-flow: column` + `overflow-x` para carruseles.
> - `grid-auto-rows: minmax(<min>, auto)` para filas que no se aplastan pero crecen con el contenido.

## Errores comunes

> [!warning] Trampas
> - **Filas implícitas sin altura definida**: pueden quedar más pequeñas de lo esperado (usa `grid-auto-rows`).
> - **`dense` que reordena** visualmente respecto al foco/lectura (accesibilidad).
> - **Confundir explícito con implícito**: `grid-template-rows` define filas explícitas; `grid-auto-rows`, las implícitas.

## Notas relacionadas

- [[03 grid-template-columns y rows]] — las pistas explícitas.
- [[08 Span]] — los items grandes que dejan huecos.
- [[08 order]] — el problema de accesibilidad que comparte `dense`.
