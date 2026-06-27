---
title: <thead>, <tbody>, <tfoot> — Secciones de la tabla
aliases:
  - thead
  - tbody
  - tfoot
  - table sections
tags:
  - html
  - api/elemento
  - tablas
elemento: tbody
categoria: ninguna
rol_implicito: rowgroup
vacio: false
draft: false
order: 7
---

# Secciones (thead, tbody, tfoot)

> [!definicion]
> Las tres secciones dividen la tabla en cabecera, cuerpo y pie: `<thead>` agrupa las filas de encabezado, `<tbody>` las filas de datos y `<tfoot>` las de resumen (totales). Aportan estructura semántica y habilitan comportamientos como repetir la cabecera al imprimir.

```html
<table>
  <thead>
    <tr><th scope="col">Producto</th><th scope="col">Importe</th></tr>
  </thead>
  <tbody>
    <tr><td>Camiseta</td><td>9,99 €</td></tr>
    <tr><td>Pantalón</td><td>19,99 €</td></tr>
  </tbody>
  <tfoot>
    <tr><th scope="row">Total</th><td>29,98 €</td></tr>
  </tfoot>
</table>
```

## Las tres secciones

| Sección | Contiene | Rol típico |
|---------|----------|------------|
| `<thead>` | Filas de encabezado de columna | Etiquetas de las columnas |
| `<tbody>` | Filas de datos | El contenido principal |
| `<tfoot>` | Filas de pie | Totales, sumas, resúmenes |

## tbody implícito

Si no se declaran secciones y las filas van sueltas dentro del `<table>`, el navegador crea un **`<tbody>` implícito** que las envuelve. Por eso una tabla sin `<tbody>` explícito sigue teniendo uno en el DOM. Declararlas explícitamente es, aun así, mejor práctica.

## Beneficios concretos

> [!info] Por qué usar las secciones
> No son decorativas; habilitan comportamientos reales:
> - **Impresión**: el contenido de `<thead>` (y `<tfoot>`) se **repite en cada página** cuando una tabla larga se imprime en varias hojas, para no perder el contexto de las columnas.
> - **Scroll**: con CSS (`position: sticky` en `thead th`) la cabecera puede quedar fija al desplazar una tabla larga.
> - **Estilo**: permiten reglas claras (`tbody tr:nth-child(odd)` para el zebra striping sin afectar a la cabecera).
> - **Semántica**: cada sección es un `rowgroup`, lo que ayuda a la asistencia a entender la estructura.

## Orden en el código

`<tfoot>` puede escribirse **antes** o después del `<tbody>` en el marcado; el navegador lo renderiza siempre al final. Históricamente se ponía antes para que el navegador pudiera mostrar el pie antes de cargar todas las filas, aunque hoy el orden natural (thead → tbody → tfoot) es el habitual y recomendado.

## Cabecera fija al hacer scroll

```css
thead th {
  position: sticky;
  top: 0;
  background: #181825;
}
```

Combinado con `<thead>`, mantiene las etiquetas de columna visibles mientras se recorre una tabla larga.

## Buenas prácticas

> [!tip] Recomendaciones
> - Declara `<thead>` y `<tbody>` explícitamente, aunque el `<tbody>` sea implícito.
> - Usa `<tfoot>` para totales y resúmenes, no para datos normales.
> - Aprovecha las secciones para `sticky` headers e impresión de tablas largas.

## Errores comunes

> [!warning] Trampas
> - **No usar `<thead>`** y poner la cabecera como una fila más del `<tbody>`: pierde la repetición al imprimir y la semántica.
> - **Meter datos en `<tfoot>`**: es para resúmenes, no para contenido normal.
> - **Varias `<thead>`/`<tfoot>`**: solo se permite una de cada por tabla (varios `<tbody>`, en cambio, sí son válidos).

## Notas relacionadas

- [[01 Contenedor de Tabla (table)]] — el contenedor que agrupa las secciones.
- [[02 Fila de Tabla (tr)]] — las filas que viven en cada sección.
- [[03 Celda de Encabezado (th)]] — las celdas de la cabecera.
