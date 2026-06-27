---
title: <caption> — Título de la tabla
aliases:
  - caption
  - table caption
tags:
  - html
  - api/elemento
  - tablas
elemento: caption
categoria: ninguna
rol_implicito: caption
vacio: false
draft: false
order: 8
---

# Título de Tabla (caption)

> [!definicion]
> `<caption>` es el **título** de una tabla: describe de qué tratan sus datos. Debe ser el **primer hijo** del [[01 Contenedor de Tabla (table) | `<table>`]]. El navegador lo muestra centrado sobre la tabla por defecto, y es clave para la accesibilidad.

```html
<table>
  <caption>Ventas mensuales del primer trimestre de 2026</caption>
  <thead>
    <tr><th scope="col">Mes</th><th scope="col">Ventas</th></tr>
  </thead>
  …
</table>
```

## Por qué importa para la accesibilidad

> [!info] El caption es el "nombre" de la tabla
> Para un lector de pantalla, el `<caption>` es el **nombre accesible** de la tabla: lo anuncia al entrar en ella ("Tabla: Ventas mensuales del primer trimestre, 4 filas, 2 columnas"). Sin caption, el usuario llega a una tabla sin saber qué contiene y debe deducirlo de las celdas. Es el equivalente, para una tabla, del `alt` de una imagen o el `<title>` de la página: el contexto que la hace comprensible sin verla.

## Posición y contenido

| Regla | Detalle |
|-------|---------|
| Posición | **Primer hijo** del `<table>`, antes de `<colgroup>`/`<thead>` |
| Cantidad | Solo **uno** por tabla |
| Contenido | Texto y elementos de fraseo (puede llevar `<strong>`, enlaces) |

## Estilarlo

El `<caption>` se puede posicionar (arriba o abajo) y estilar con CSS:

```css
caption {
  caption-side: bottom;   /* o "top" (por defecto) */
  font-weight: 600;
  padding: 0.5rem;
  text-align: left;
  color: #bac2de;
}
```

`caption-side` es una propiedad específica para decidir si el título va sobre o bajo la tabla.

## caption vs. otros mecanismos de etiquetado

| Mecanismo | Cuándo |
|-----------|--------|
| `<caption>` | El título de la tabla, visible y accesible (preferido) |
| `aria-label` en `<table>` | Nombre accesible sin texto visible |
| `aria-labelledby` | Apuntar a un encabezado externo que titula la tabla |

`<caption>` es el método preferido porque es visible para todos y no depende de ARIA.

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon un `<caption>` descriptivo en **toda** tabla de datos.
> - Colócalo como primer hijo del `<table>`.
> - Si por diseño no quieres que se vea, prefiere ocultarlo accesiblemente (clase `sr-only`) antes que omitirlo: la asistencia lo sigue necesitando.

## Errores comunes

> [!warning] Trampas
> - **Omitir el `<caption>`**: la tabla queda sin nombre para la asistencia.
> - **Ponerlo fuera de su posición** (no como primer hijo): marcado inválido.
> - **Usar un `<p>` encima de la tabla** como "título": se ve, pero no se asocia semánticamente a la tabla.

## Notas relacionadas

- [[01 Contenedor de Tabla (table)]] — el contenedor del que `<caption>` es primer hijo.
- [[03 Celda de Encabezado (th)]] — los encabezados que, junto al caption, dan contexto.
- [[03 Contenido Oculto Visualmente (sr-only)]] — cómo ocultar el caption sin perder accesibilidad.
