---
title: <colgroup> y <col> — Agrupar y estilar columnas
aliases:
  - colgroup
  - col
  - column group
tags:
  - html
  - api/elemento
  - tablas
elemento: colgroup
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
---

# Agrupación de Columnas (colgroup, col)

> [!definicion]
> `<colgroup>` define un grupo de columnas y `<col>` representa una columna individual dentro de él. Sirven para **aplicar estilos o atributos a columnas enteras** de una vez, sin tener que tocar cada celda. Van al principio del [[01 Contenedor de Tabla (table) | `<table>`]], tras el `<caption>`.

```html
<table>
  <caption>Precios</caption>
  <colgroup>
    <col style="background: #1e1e2e" />
    <col span="2" style="background: #181825" />
  </colgroup>
  <tr><th>Producto</th><th>Precio</th><th>IVA</th></tr>
  <tr><td>Camiseta</td><td>9,99</td><td>21%</td></tr>
</table>
```

## El atributo span

`span` indica cuántas columnas abarca un `<col>` o `<colgroup>`:

```html
<colgroup>
  <col />              <!-- columna 1 -->
  <col span="2" />     <!-- columnas 2 y 3 juntas -->
</colgroup>
```

## Para qué sirve (y para qué no)

El uso principal es **estilar columnas completas**: fondo, ancho, borde. Es más limpio que añadir una clase a cada celda de la columna.

| Propiedad CSS | ¿Funciona vía col? |
|---------------|--------------------|
| `background` | Sí |
| `width` | Sí |
| `border` | Sí (con matices según `border-collapse`) |
| `visibility: collapse` | Sí (oculta la columna) |
| `color`, `font`, `padding` | **No** (se aplican a la celda, no a la columna) |

> [!warning] col solo estila ciertas propiedades
> `<col>` afecta a un conjunto **limitado** de propiedades (sobre todo fondo y ancho). Propiedades de texto como `color`, `font-family` o `padding` **no** se heredan desde `<col>`: hay que aplicarlas a `<td>`/`<th>`. Es la limitación que más sorprende: estilar la columna no es como estilar todas sus celdas.

## Ejemplo: anchos de columna

```html
<table>
  <colgroup>
    <col style="width: 60%" />
    <col style="width: 20%" />
    <col style="width: 20%" />
  </colgroup>
  …
</table>
```

Definir los anchos en `<col>` mantiene la tabla coherente sin repetir `width` en cada fila.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<colgroup>`/`<col>` para fondo y ancho de columnas enteras.
> - Para estilos de texto y espaciado, aplica reglas a `<td>`/`<th>` (p. ej. `td:nth-child(2)`).
> - Coloca el `<colgroup>` justo tras el `<caption>`, antes de las filas.

## Errores comunes

> [!warning] Trampas
> - **Esperar que `<col>` estile texto o padding**: no lo hace; usa las celdas.
> - **Colocar `<colgroup>` fuera de su sitio**: debe ir al inicio de la tabla.
> - **Descuadre con `span`**: el total de columnas declaradas debe coincidir con las reales.

## Notas relacionadas

- [[01 Contenedor de Tabla (table)]] — donde se declara el `<colgroup>`.
- [[05 Fusión de Celdas (colspan, rowspan)]] — fusionar celdas, distinto de agrupar columnas.
