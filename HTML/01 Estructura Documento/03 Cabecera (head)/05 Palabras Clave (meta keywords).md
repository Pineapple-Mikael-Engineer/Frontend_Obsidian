---
title: <meta keywords> — Palabras clave (obsoleta para SEO)
aliases:
  - meta keywords
tags:
  - html
  - api/elemento
  - metadatos
elemento: meta
categoria: metadatos
rol_implicito: ninguno
vacio: true
draft: false
---

# Palabras Clave (meta keywords)

> [!definicion]
> `<meta name="keywords">` enumeraba las palabras clave de la página. **Google la ignora desde
> 2009** por el abuso de spam (keyword stuffing). Hoy es marcado muerto para posicionamiento.

```html
<meta name="keywords" content="recetas, tarta, manzana, postre" />
```

> [!warning] No invertir esfuerzo en ella
> Ningún buscador relevante (Google, Bing) la usa para ranking. Rellenarla no perjudica, pero
> tampoco aporta. El esfuerzo de SEO va a [[03 Título del Documento (title) | `<title>`]],
> [[04 Descripción (meta description) | `description`]], contenido real y datos estructurados.

> [!info] Por qué sigue en el temario
> Se documenta para **reconocerla y descartarla**: aparece en plantillas y tutoriales antiguos, y
> conviene saber que su ausencia no es un error. Es el ejemplo canónico de metadato deprecado por
> abuso.

## Notas relacionadas

- [[04 Descripción (meta description)]] — el `meta` que **sí** sigue vigente.
- [[09 Metadatos y SEO/index]] — qué metadatos importan hoy.
