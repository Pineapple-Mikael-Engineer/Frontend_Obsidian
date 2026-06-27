---
title: break-inside — Evitar que un elemento se parta
aliases:
  - break-inside
tags:
  - css
  - api/propiedad
  - layout
propiedad: break-inside
grupo: layout
valor_inicial: auto
hereda: false
animable: false
draft: false
order: 4
---

# break-inside

> [!definicion]
> `break-inside` controla si un elemento puede **partirse** entre dos columnas (en multicolumna) o entre dos páginas (al imprimir). `break-inside: avoid` impide que un elemento quede cortado a la mitad, manteniéndolo entero en una sola columna o página.

```css
.tarjeta { break-inside: avoid; }   /* no se parte entre columnas */
```

## Valores

| Valor | Efecto |
|-------|--------|
| `auto` | Permite la partición (por defecto) |
| `avoid` | Evita partir el elemento (lo mantiene entero) |
| `avoid-column` | Evita partir entre **columnas** |
| `avoid-page` | Evita partir entre **páginas** (impresión) |

## El problema: tarjetas cortadas entre columnas

> [!warning] Sin break-inside, los elementos se parten
> En un layout multicolumna, un elemento (una tarjeta, una cita, una imagen con pie) puede quedar **cortado**: la mitad al final de una columna y la otra mitad al principio de la siguiente. Se ve roto. `break-inside: avoid` lo mantiene entero:
> ```css
> .columnas { columns: 16rem; }
> .columnas .item {
>   break-inside: avoid;   /* cada item se queda completo en una columna */
> }
> ```

## El caso de la impresión

> [!tip] break-inside para imprimir bien
> `break-inside: avoid` es igual de útil al **imprimir**: evita que una tabla, una imagen con su pie, o un bloque de código se corten entre dos páginas. Es parte de los estilos de impresión (`@media print`):
> ```css
> @media print {
>   figure, table, .no-cortar { break-inside: avoid; }
> }
> ```

## Las propiedades relacionadas

`break-inside` forma parte de una familia para controlar las roturas:

| Propiedad | Controla |
|-----------|----------|
| `break-inside` | Si el elemento **se parte** por dentro |
| `break-before` | Forzar/evitar rotura **antes** del elemento |
| `break-after` | Forzar/evitar rotura **después** |

`break-before: column` fuerza que un elemento empiece en una **columna nueva**; `break-before: page` (impresión) en una página nueva.

## Buenas prácticas

> [!tip] Recomendaciones
> - `break-inside: avoid` en elementos que no deben cortarse (tarjetas, citas, imágenes con pie).
> - Aplícalo también en `@media print` para tablas, figuras y bloques de código.
> - `break-before: column`/`page` para forzar inicios de columna/página.
> - Recuerda que solo importa en contextos fragmentables (multicolumna, impresión).

## Errores comunes

> [!warning] Trampas
> - **Olvidarlo** en multicolumna: las tarjetas se parten entre columnas.
> - **Aplicarlo a todo** (puede dejar columnas muy desiguales si los elementos son grandes).
> - **Esperar efecto** fuera de multicolumna/impresión: no hace nada en layout normal.

## Notas relacionadas

- [[01 column-count y column-width]] — el contexto multicolumna.
- [[02 box-decoration-break]] — cómo se decora un elemento partido.
- [[04 CSS Externo (link)]] — los estilos de impresión (`media="print"`).
