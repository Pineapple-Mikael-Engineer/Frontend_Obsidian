---
title: <table> — Contenedor de tabla
aliases:
  - table element
tags:
  - html
  - api/elemento
  - tablas
elemento: table
categoria: flujo
rol_implicito: table
vacio: false
draft: false
order: 1
---

# Contenedor de Tabla (table)

> [!definicion]
> `<table>` es el contenedor de una tabla de **datos tabulares**. Agrupa todas las piezas: el título [[08 Título de Tabla (caption) | `<caption>`]], las secciones [[07 Secciones (thead, tbody, tfoot) | `<thead>`/`<tbody>`/`<tfoot>`]], las filas [[02 Fila de Tabla (tr) | `<tr>`]] y las celdas.

```html
<table>
  <caption>Inventario</caption>
  <thead>
    <tr><th scope="col">Producto</th><th scope="col">Stock</th></tr>
  </thead>
  <tbody>
    <tr><td>Camiseta</td><td>42</td></tr>
    <tr><td>Pantalón</td><td>17</td></tr>
  </tbody>
</table>
```

## Orden de los hijos

Los elementos hijos de `<table>` deben aparecer en un orden concreto:

| Orden | Elemento | Obligatorio |
|-------|----------|-------------|
| 1 | `<caption>` | No (pero recomendado) |
| 2 | `<colgroup>` | No |
| 3 | `<thead>` | No |
| 4 | `<tbody>` | Implícito si se omite |
| 5 | `<tfoot>` | No |

El navegador es tolerante (crea un `<tbody>` implícito si las filas van sueltas), pero respetar el orden produce un DOM predecible y accesible.

## Tabla mínima vs. tabla completa

```html
<!-- Mínima (válida pero pobre en accesibilidad) -->
<table>
  <tr><td>A</td><td>B</td></tr>
</table>

<!-- Completa (recomendada) -->
<table>
  <caption>Título descriptivo</caption>
  <thead><tr><th scope="col">Col 1</th><th scope="col">Col 2</th></tr></thead>
  <tbody><tr><td>A</td><td>B</td></tr></tbody>
</table>
```

## Estilizar tablas

Las tablas tienen propiedades CSS específicas para el manejo de bordes:

```css
table {
  border-collapse: collapse;   /* fusiona bordes adyacentes en uno */
  width: 100%;
}
th, td {
  border: 1px solid #45475a;
  padding: 0.5rem;
  text-align: left;
}
```

`border-collapse: collapse` es casi siempre lo que se quiere: une los bordes de celdas contiguas en una sola línea, en vez de dejar el doble borde por defecto.

## Tablas responsivas

> [!tip] Tablas anchas en móvil
> Una tabla con muchas columnas desborda en móvil. La solución más robusta y accesible es envolverla en un contenedor con scroll horizontal, en lugar de reestructurar el marcado:
> ```html
> <div style="overflow-x: auto">
>   <table>…</table>
> </div>
> ```
> Reorganizar visualmente las celdas con CSS (`display: block`) puede romper la asociación celda–encabezado para lectores de pantalla; el scroll preserva la semántica.

## Buenas prácticas

> [!tip] Recomendaciones
> - Incluye siempre un `<caption>` y usa `<thead>`/`<tbody>` para estructurar.
> - `border-collapse: collapse` para bordes limpios.
> - Para tablas anchas, scroll horizontal en un contenedor; no rompas el marcado.
> - Solo para datos tabulares reales, nunca para layout.

## Errores comunes

> [!warning] Trampas
> - **Usar `<table>` para maquetar** la página: obsoleto y dañino para la accesibilidad.
> - **Omitir `<th>`** y usar solo `<td>`: la tabla pierde la asociación de encabezados.
> - **No envolver tablas anchas**: desbordan en móvil.

## Notas relacionadas

- [[02 Fila de Tabla (tr)]] — las filas que contiene.
- [[07 Secciones (thead, tbody, tfoot)]] — la estructura interna recomendada.
- [[08 Título de Tabla (caption)]] — el título de la tabla.
