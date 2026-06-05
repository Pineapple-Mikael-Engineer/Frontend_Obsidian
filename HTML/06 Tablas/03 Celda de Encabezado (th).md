---
title: <th> — Celda de encabezado
aliases:
  - th
  - table header cell
tags:
  - html
  - api/elemento
  - tablas
elemento: th
categoria: ninguna
rol_implicito: columnheader
vacio: false
draft: false
---

# Celda de Encabezado (th)

> [!definicion]
> `<th>` (table header) es una celda que actúa como **encabezado** de una columna o de una fila: etiqueta los datos relacionados. El navegador la muestra en **negrita y centrada** por defecto, pero su valor real es semántico: asocia cada dato con su encabezado para las tecnologías de asistencia.

```html
<tr>
  <th scope="col">Producto</th>
  <th scope="col">Precio</th>
</tr>
```

## El atributo scope: la clave de la accesibilidad

`scope` indica **qué celdas** encabeza un `<th>`. Es lo que permite a un lector de pantalla anunciar "Precio: 9,99 €" al llegar a una celda de datos.

| `scope` | El `<th>` encabeza |
|---------|--------------------|
| `col` | Toda su columna |
| `row` | Toda su fila |
| `colgroup` | Un grupo de columnas |
| `rowgroup` | Un grupo de filas |

```html
<table>
  <thead>
    <tr>
      <th scope="col">Producto</th>
      <th scope="col">Stock</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Camiseta</th>
      <td>42</td>
    </tr>
  </tbody>
</table>
```

Aquí "Camiseta" es encabezado de su fila (`scope="row"`) y "Stock" de su columna (`scope="col"`). Un lector anuncia la celda `42` como "Camiseta, Stock, 42".

## Otros atributos

| Atributo | Descripción |
|----------|-------------|
| `scope` | A qué celdas encabeza (`col`, `row`, `colgroup`, `rowgroup`) |
| `abbr` | Versión abreviada del encabezado para la asistencia |
| `colspan` / `rowspan` | Fusión de celdas (ver nota dedicada) |
| `headers` | Asocia con otros `<th>` por `id` (tablas complejas) |

## Tablas complejas: headers e id

Para tablas con varios niveles de encabezado donde `scope` no basta, se usan `id` en los `<th>` y `headers` en las celdas para asociarlas explícitamente:

```html
<th id="t1">Trimestre</th>
…
<td headers="t1">Q1</td>
```

Es verboso pero permite describir relaciones que `scope` no puede expresar.

## Accesibilidad

> [!info] th vs. td: la diferencia que importa
> Visualmente, un `<th>` se distingue de un `<td>` por la negrita y el centrado, que se pueden replicar con CSS. Pero su **valor semántico no**: un `<th scope>` le dice al lector de pantalla qué etiqueta cada dato. Una tabla con solo `<td>` —aunque la primera fila "parezca" cabecera por el estilo— deja a quien no ve la pantalla sin contexto: oye números sueltos sin saber a qué columna pertenecen.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<th>` para **toda** celda de encabezado, de columna y de fila.
> - Añade `scope="col"`/`scope="row"` siempre: es barato y transforma la accesibilidad.
> - Para encabezados largos, usa `abbr` con una forma corta para la asistencia.
> - Reserva `id`/`headers` para tablas complejas donde `scope` no alcanza.

## Errores comunes

> [!warning] Trampas
> - **Cabecera con `<td>` y negrita CSS**: parece encabezado pero no lo es semánticamente.
> - **`<th>` sin `scope`**: pierde gran parte de su utilidad para la asistencia.
> - **Abusar de `headers`/`id`** en tablas simples donde `scope` bastaría.

## Notas relacionadas

- [[04 Celda de Datos (td)]] — la celda de datos que el `<th>` encabeza.
- [[05 Fusión de Celdas (colspan, rowspan)]] — fusionar encabezados.
- [[08 Título de Tabla (caption)]] — el título global de la tabla.
