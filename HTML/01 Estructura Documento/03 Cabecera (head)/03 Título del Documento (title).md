---
title: <title> — Título del documento
aliases:
  - title element
  - page title
tags:
  - html
  - api/elemento
  - metadatos
elemento: title
categoria: metadatos
rol_implicito: ninguno
vacio: false
draft: false
---

# Título del Documento (title)

> [!definicion]
> `<title>` define el **título de la página**: el texto que aparece en la pestaña del navegador, en
> los marcadores, en el historial y como **titular azul** en los resultados de Google. Es el único
> elemento del [[index | `<head>`]] cuyo contenido el usuario percibe directamente.

```html
<head>
  <title>Tarta de manzana fácil — Recetas de la Abuela</title>
</head>
```

Es **obligatorio**: un documento HTML válido debe tener exactamente un `<title>` con texto. Solo
admite texto plano (sin etiquetas anidadas).

## Buenas prácticas de redacción

| Práctica | Razón |
|----------|-------|
| Lo específico primero | La pestaña y Google truncan; el dato útil debe ir al inicio |
| Patrón `Página — Sitio` | Identifica la página y la marca de un vistazo |
| 50–60 caracteres | Google corta alrededor de los 60 |
| Único por página | Páginas con el mismo título confunden a buscadores y usuarios |

> [!info] Peso en SEO y accesibilidad
> El `<title>` es uno de los factores SEO on-page más fuertes y lo **primero** que anuncia un
> lector de pantalla al cargar la página. Un buen título orienta tanto al buscador como a la
> persona ciega que navega entre pestañas.

> [!warning] No confundir con el encabezado h1
> `<title>` vive en el `<head>` (metadato, pestaña); [[01 Encabezados Jerárquicos (h1-h6) | `<h1>`]]
> vive en el `<body>` (contenido visible, titular de la página). Pueden parecerse pero cumplen roles
> distintos y no deben asumirse iguales: el `<title>` suele añadir el nombre del sitio.

## Notas relacionadas

- [[04 Descripción (meta description)]] — el texto que acompaña al título en los buscadores.
- [[01 Encabezados Jerárquicos (h1-h6)]] — el titular visible, no confundir.
