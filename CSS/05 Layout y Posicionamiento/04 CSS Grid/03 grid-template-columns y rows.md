---
title: grid-template-columns y rows — Definir las pistas
aliases:
  - grid-template-columns
  - grid-template-rows
tags:
  - css
  - api/propiedad
  - layout
propiedad: grid-template-columns
grupo: grid
valor_inicial: none
hereda: false
animable: false
draft: false
---

# grid-template-columns y rows

> [!definicion]
> `grid-template-columns` y `grid-template-rows` definen las **pistas** de la rejilla: cuántas columnas/filas hay y qué tamaño tiene cada una. Es la propiedad central de Grid: la lista de valores determina el número y el ancho/alto de las pistas.

```css
.rejilla {
  display: grid;
  grid-template-columns: 200px 1fr 1fr;   /* 3 columnas */
  grid-template-rows: auto 1fr auto;       /* 3 filas */
}
```

## Cada valor es una pista

> [!info] Tantos valores, tantas pistas
> El número de valores que escribes = el número de pistas. `grid-template-columns: 1fr 1fr 1fr` crea **tres** columnas; cada valor define el tamaño de una:
> ```css
> grid-template-columns: 200px 1fr;   /* 2 columnas: una fija de 200px, otra flexible */
> ```

## Tipos de tamaño de pista

| Valor | Tamaño |
|-------|--------|
| `200px`, `10rem` | Fijo |
| `1fr` | Una fracción del espacio sobrante (ver `fr` y `repeat()`) |
| `auto` | El tamaño del contenido |
| `min-content` / `max-content` | Según el contenido |
| `minmax(min, max)` | Entre un mínimo y un máximo |
| `repeat(n, ...)` | Repetir un patrón de pistas |
| `%` | Porcentaje del contenedor |

## Mezclar fijo y flexible

El patrón más común combina pistas **fijas** (una barra lateral) con pistas **flexibles** (`1fr` para el contenido):

```css
/* Barra lateral fija + contenido que ocupa el resto */
.layout { display: grid; grid-template-columns: 250px 1fr; }

/* Contenido centrado con márgenes flexibles */
.centrado { display: grid; grid-template-columns: 1fr min(65rem, 100%) 1fr; }
```

## grid-auto-rows para filas implícitas

> [!info] Las filas suelen no declararse
> A menudo se definen las **columnas** y se deja que las filas se creen automáticamente según el contenido (filas implícitas). Su tamaño lo controla [[10 Grid Implícito (auto-rows, auto-columns, auto-flow) | `grid-auto-rows`]]:
> ```css
> .galeria {
>   display: grid;
>   grid-template-columns: repeat(3, 1fr);   /* columnas explícitas */
>   grid-auto-rows: 200px;                    /* cada fila que se cree, 200px */
> }
> ```

## El shorthand grid-template

`grid-template` combina filas, columnas y áreas:

```css
grid-template:
  "cabecera" auto
  "contenido" 1fr
  "pie" auto
  / 1fr;   /* la / separa la definición de columnas */
```

Es potente pero denso; muchos prefieren las propiedades por separado.

## Buenas prácticas

> [!tip] Recomendaciones
> - Define las columnas explícitamente; deja las filas implícitas con `grid-auto-rows` si aplica.
> - Mezcla pistas fijas (`px`/`rem`) y flexibles (`1fr`) para layouts robustos.
> - Usa `repeat()` para no repetir valores (`repeat(3, 1fr)` en vez de `1fr 1fr 1fr`).
> - Para columnas con mínimo y máximo, `minmax()`.

## Errores comunes

> [!warning] Trampas
> - **Olvidar definir columnas**: Grid apila en una sola columna.
> - **Repetir valores a mano** en vez de usar `repeat()`.
> - **Pistas en `%`** sin contar el `gap` (a diferencia de `fr`, que sí lo descuenta).

## Notas relacionadas

- [[04 Unidad fr y repeat()]] — `1fr` y `repeat()`, esenciales aquí.
- [[05 minmax() y fit-content()]] — pistas con límites.
- [[10 Grid Implícito (auto-rows, auto-columns, auto-flow)]] — las filas que se crean solas.
