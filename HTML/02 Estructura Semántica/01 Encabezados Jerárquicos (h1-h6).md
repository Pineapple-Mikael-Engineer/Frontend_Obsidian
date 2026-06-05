---
title: <h1>–<h6> — Encabezados jerárquicos
aliases:
  - headings
  - h1 h2 h3
tags:
  - html
  - api/elemento
  - semantica
elemento: h1
categoria: flujo
rol_implicito: heading
vacio: false
draft: false
---

# Encabezados Jerárquicos (h1-h6)

> [!definicion]
> `<h1>` a `<h6>` son los **títulos** de la página, ordenados por importancia: `<h1>` es el más
> relevante, `<h6>` el menos. Forman el **esquema** del documento, equivalente al índice de un libro.

```html
<h1>Guía de jardinería</h1>
  <h2>Plantas de interior</h2>
    <h3>Riego</h3>
    <h3>Luz</h3>
  <h2>Plantas de exterior</h2>
```

## Reglas de jerarquía

| Regla | Razón |
|-------|-------|
| Un solo `<h1>` por página | Es el título principal; varios confunden el esquema |
| No saltar niveles | De `<h2>` se baja a `<h3>`, no a `<h4>` |
| El nivel indica jerarquía, **no tamaño** | El tamaño se controla con CSS, no eligiendo otra etiqueta |

La anidación correcta construye un árbol: cada encabezado abre una sección que pertenece al
encabezado de nivel superior anterior.

> [!info] Navegación por encabezados
> Los lectores de pantalla permiten **saltar de encabezado en encabezado** y listar el esquema
> completo (tecla rápida en NVDA/VoiceOver). Una jerarquía bien formada es un mapa de la página para
> usuarios ciegos; una rota los desorienta.

> [!warning] No elegir el nivel por su apariencia
> Usar `<h4>` "porque se ve del tamaño que quiero" rompe el esquema. Si un título de nivel 2 debe
> verse pequeño, se mantiene `<h2>` y se ajusta con `font-size` en CSS. La etiqueta es semántica; el
> tamaño es presentación.

> [!tip] No confundir con el title
> [[03 Título del Documento (title) | `<title>`]] (pestaña, metadato) y `<h1>` (titular visible) son
> distintos. Suelen parecerse, pero el `<title>` añade el nombre del sitio y el `<h1>` no.

## Notas relacionadas

- [[02 Agrupación de Título (hgroup)]] — agrupar un `<h1>` con su subtítulo.
- [[01 HTML Semántico como Base]] — los encabezados como columna vertebral de la accesibilidad.
