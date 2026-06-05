---
title: colspan y rowspan — Fusión de celdas
aliases:
  - colspan
  - rowspan
  - cell merge
tags:
  - html
  - api/atributo
  - tablas
elemento: td
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
---

# Fusión de Celdas (colspan, rowspan)

> [!definicion]
> `colspan` y `rowspan` permiten que una celda ([[04 Celda de Datos (td) | `<td>`]] o [[03 Celda de Encabezado (th) | `<th>`]]) ocupe **varias columnas o filas**, fusionando el espacio. `colspan` extiende horizontalmente; `rowspan`, verticalmente.

```html
<table>
  <tr>
    <th colspan="2">Datos personales</th>   <!-- ocupa 2 columnas -->
  </tr>
  <tr>
    <td>Nombre</td>
    <td>Ana</td>
  </tr>
</table>
```

## colspan: fusión horizontal

`colspan="n"` hace que la celda abarque `n` columnas. Útil para encabezados que agrupan varias columnas:

```html
<tr>
  <th colspan="3">Primer trimestre</th>
</tr>
<tr>
  <th>Enero</th><th>Febrero</th><th>Marzo</th>
</tr>
```

## rowspan: fusión vertical

`rowspan="n"` hace que la celda abarque `n` filas. Útil cuando un valor se repite en filas consecutivas:

```html
<tr>
  <th rowspan="2">Europa</th>
  <td>España</td>
</tr>
<tr>
  <td>Francia</td>   <!-- "Europa" ya ocupa esta fila desde arriba -->
</tr>
```

> [!info] La celda fusionada "desaparece" de las filas siguientes
> Con `rowspan="2"`, la celda de "Europa" se extiende a la fila de abajo. Por eso esa segunda fila tiene **una celda menos** en el marcado: el hueco ya lo ocupa la celda fusionada desde arriba. Este es el origen de la mayoría de tablas descuadradas.

## El recuento de celdas debe cuadrar

> [!warning] La cuenta es la causa nº 1 de tablas rotas
> Al fusionar, hay que recalcular cuántas celdas explícitas necesita cada fila. Una rejilla de 3 columnas:
> - Una fila con un `colspan="2"` + un `<td>` normal = 3 columnas ✅ (2 celdas en el marcado).
> - Una fila con `rowspan` heredado de arriba lleva una celda menos.
>
> Si la cuenta no cuadra (celdas de más o de menos), la tabla se descuadra visualmente y la asociación celda–encabezado se rompe para la asistencia.

## Ejemplo combinado

```html
<table border="1">
  <tr>
    <th rowspan="2">Región</th>
    <th colspan="2">2026</th>
  </tr>
  <tr>
    <th>Q1</th><th>Q2</th>
  </tr>
  <tr>
    <td>Norte</td><td>120</td><td>150</td>
  </tr>
</table>
```

## Accesibilidad

> [!warning] Las fusiones complican la lectura
> Las celdas fusionadas hacen las tablas más difíciles de interpretar para los lectores de pantalla. En tablas con fusiones complejas, conviene usar `id`/`headers` (ver [[03 Celda de Encabezado (th) | `<th>`]]) para dejar explícita la asociación, ya que `scope` puede no bastar. Como regla, mantén las fusiones al mínimo necesario.

## Buenas prácticas

> [!tip] Recomendaciones
> - Recalcula el número de celdas de cada fila tras cada fusión.
> - Usa fusiones solo cuando reflejen la estructura real de los datos.
> - En tablas con fusiones complejas, refuerza con `id`/`headers`.
> - `colspan`/`rowspan` aceptan solo enteros positivos; no uses `0`.

## Errores comunes

> [!warning] Trampas
> - **Cuenta de celdas incorrecta**: la causa más habitual de tablas descuadradas.
> - **Olvidar que `rowspan` quita una celda** de las filas siguientes.
> - **Abusar de fusiones** hasta hacer la tabla ininteligible para la asistencia.

## Notas relacionadas

- [[04 Celda de Datos (td)]] · [[03 Celda de Encabezado (th)]] — las celdas que se fusionan.
- [[06 Agrupación de Columnas (colgroup, col)]] — agrupar columnas sin fusionar celdas.
