---
title: <td> — Celda de datos
aliases:
  - td
  - table data cell
tags:
  - html
  - api/elemento
  - tablas
elemento: td
categoria: ninguna
rol_implicito: cell
vacio: false
draft: false
order: 4
---

# Celda de Datos (td)

> [!definicion]
> `<td>` (table data) es una celda que contiene un **dato** de la tabla: el valor que se cruza en una fila y una columna. Es la unidad básica de contenido de una tabla y admite cualquier contenido de flujo (texto, imágenes, enlaces, incluso otra tabla).

```html
<tr>
  <th scope="row">Camiseta</th>
  <td>42 unidades</td>
  <td>9,99 €</td>
</tr>
```

## td vs. th

| | `<td>` | `<th>` |
|--|--------|--------|
| Contiene | Un dato | Un encabezado |
| Estilo por defecto | Normal, alineado a la izquierda | Negrita, centrado |
| Rol semántico | `cell` | `columnheader` / `rowheader` |

La regla: si la celda **etiqueta** otros datos, es [[03 Celda de Encabezado (th) | `<th>`]]; si **es** un dato, es `<td>`.

## Atributos

| Atributo | Descripción |
|----------|-------------|
| `colspan` | Cuántas columnas ocupa la celda (fusión horizontal) |
| `rowspan` | Cuántas filas ocupa la celda (fusión vertical) |
| `headers` | `id` de los `<th>` que encabezan esta celda (tablas complejas) |

La fusión con `colspan`/`rowspan` tiene su propia nota: [[05 Fusión de Celdas (colspan, rowspan) | fusión de celdas]].

## Contenido rico en celdas

Un `<td>` puede contener mucho más que texto:

```html
<td>
  <a href="/producto/1">
    <img src="thumb.jpg" alt="Camiseta roja" width="40" />
  </a>
</td>
<td><button>Comprar</button></td>
```

## Alineación de datos numéricos

> [!tip] Números a la derecha
> Por convención, los datos numéricos se alinean a la derecha para comparar magnitudes de un vistazo (las unidades, decenas y centenas quedan en columna). El texto, a la izquierda:
> ```css
> td.numero { text-align: right; font-variant-numeric: tabular-nums; }
> ```
> `tabular-nums` hace que todos los dígitos ocupen el mismo ancho, alineando los decimales perfectamente.

## Celdas vacías

Una celda sin dato se deja como `<td></td>`. Para indicar explícitamente "sin dato" de forma accesible, es mejor un contenido claro ("—", "N/D") que una celda totalmente vacía, que un lector de pantalla puede saltarse sin avisar.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<td>` solo para datos; los encabezados son `<th>`.
> - Alinea números a la derecha con `tabular-nums` para comparabilidad.
> - Para "sin dato", pon un marcador visible ("—") en lugar de dejar la celda vacía.

## Errores comunes

> [!warning] Trampas
> - **Usar `<td>` para encabezados** con negrita CSS: rompe la asociación semántica.
> - **Distinto número de celdas por fila** sin las fusiones correspondientes.
> - **Celdas totalmente vacías** que confunden la lectura asistida.

## Notas relacionadas

- [[03 Celda de Encabezado (th)]] — la celda de encabezado, su contraparte.
- [[05 Fusión de Celdas (colspan, rowspan)]] — ocupar varias filas o columnas.
- [[02 Fila de Tabla (tr)]] — la fila que contiene las celdas.
