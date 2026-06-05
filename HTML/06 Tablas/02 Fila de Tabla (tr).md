---
title: <tr> — Fila de tabla
aliases:
  - tr
  - table row
tags:
  - html
  - api/elemento
  - tablas
elemento: tr
categoria: ninguna
rol_implicito: row
vacio: false
draft: false
---

# Fila de Tabla (tr)

> [!definicion]
> `<tr>` (table row) representa una **fila** de la tabla. Es el contenedor de las celdas: solo puede contener [[03 Celda de Encabezado (th) | `<th>`]] (encabezado) y [[04 Celda de Datos (td) | `<td>`]] (datos). Las filas se apilan verticalmente dentro de una sección de la tabla.

```html
<tr>
  <th scope="row">Camiseta</th>
  <td>42</td>
  <td>9,99 €</td>
</tr>
```

## Dónde viven las filas

Un `<tr>` va dentro de una de las tres secciones de la tabla, no suelto:

| Sección | Filas que contiene |
|---------|--------------------|
| `<thead>` | Filas de encabezado (normalmente con `<th scope="col">`) |
| `<tbody>` | Filas de datos |
| `<tfoot>` | Filas de pie (totales, resúmenes) |

Si se omiten las secciones, el navegador agrupa las filas en un `<tbody>` implícito. Más en [[07 Secciones (thead, tbody, tfoot) | secciones]].

## Solo th y td como hijos

> [!warning] Nada entre las celdas
> Un `<tr>` solo admite `<th>` y `<td>` como hijos directos. No se puede poner texto, `<div>` o cualquier otra cosa directamente en la fila:
> ```html
> <!-- ❌ texto suelto en la fila -->
> <tr>Producto: <td>Camiseta</td></tr>
>
> <!-- ✅ todo dentro de celdas -->
> <tr><th scope="row">Producto</th><td>Camiseta</td></tr>
> ```
> Todo el contenido va **dentro** de un `<th>` o `<td>`.

## El número de celdas debe ser coherente

Todas las filas deberían sumar el mismo número de columnas (contando las fusiones con [[05 Fusión de Celdas (colspan, rowspan) | `colspan`/`rowspan`]]). Una fila con celdas de más o de menos descuadra la tabla y confunde la asociación celda–encabezado.

## Estilo de filas

Las filas son el lugar natural para el "zebra striping" (filas alternas) y el resaltado al pasar el ratón:

```css
tbody tr:nth-child(odd) { background: #1e1e2e; }
tbody tr:hover { background: #313244; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Mantén el mismo número de columnas en todas las filas (contando `colspan`).
> - Pon las celdas de encabezado de fila como `<th scope="row">` al inicio de cada `<tr>` cuando aplique.
> - Coloca cada `<tr>` en la sección adecuada (`thead`/`tbody`/`tfoot`).

## Errores comunes

> [!warning] Trampas
> - **Contenido suelto** en el `<tr>` fuera de celdas.
> - **Filas con distinto número de celdas** sin las fusiones correspondientes.
> - **Mezclar encabezados y datos** sin distinguir `<th>` de `<td>`.

## Notas relacionadas

- [[03 Celda de Encabezado (th)]] · [[04 Celda de Datos (td)]] — las celdas que contiene.
- [[07 Secciones (thead, tbody, tfoot)]] — dónde se agrupan las filas.
