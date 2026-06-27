---
title: Schema Article — Artículos y noticias
aliases:
  - article schema
  - NewsArticle
tags:
  - html
  - api/concepto
  - metadatos
draft: false
order: 2
---

# Article

> [!definicion]
> El tipo `Article` (y sus subtipos `NewsArticle`, `BlogPosting`) describe un **artículo, noticia o entrada de blog**. Habilita que Google muestre el artículo con su titular, fecha y autor de forma destacada, y es elegible para carruseles de noticias.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "Cómo regar un cactus",
  "image": "https://sitio.com/cactus.jpg",
  "author": { "@type": "Person", "name": "Ana López" },
  "publisher": {
    "@type": "Organization",
    "name": "Recetas de la Abuela",
    "logo": { "@type": "ImageObject", "url": "https://sitio.com/logo.png" }
  },
  "datePublished": "2026-06-04",
  "dateModified": "2026-06-05"
}
</script>
```

## Subtipos

| `@type` | Para qué |
|---------|----------|
| `Article` | Artículo genérico |
| `NewsArticle` | Noticia (periodismo) |
| `BlogPosting` | Entrada de blog |

Elige el más específico que aplique.

## Propiedades clave

| Propiedad | Importancia |
|-----------|-------------|
| `headline` | El titular (obligatoria; máx. 110 caracteres) |
| `image` | Imagen del artículo (recomendada, idealmente varias proporciones) |
| `author` | Una `Person` u `Organization` |
| `publisher` | La `Organization` editora, con su `logo` |
| `datePublished` / `dateModified` | Fechas en formato ISO 8601 |

## El vínculo con el HTML

> [!info] Complementa, no sustituye
> Este JSON-LD complementa el marcado semántico: el artículo va en un [[06 Artículos (article) | `<article>`]] con su [[01 Encabezados Jerárquicos (h1-h6) | `<h1>`]] y su [[23 Tiempo (time) | `<time>`]]. El JSON-LD hace **explícitos** para Google esos mismos datos (autor, fecha, titular), pero deben coincidir con lo visible.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa el subtipo más específico (`BlogPosting`, `NewsArticle`).
> - `headline` ≤ 110 caracteres; coincide con el `<h1>` visible.
> - Incluye `author`, `publisher` con `logo`, y ambas fechas.
> - Aporta `image` en buena resolución (mejor varias proporciones).

## Errores comunes

> [!warning] Trampas
> - **`headline` demasiado largo**: Google lo rechaza por encima de ~110 caracteres.
> - **Sin `publisher`/`logo`**: falta un requisito para el rich result en muchos casos.
> - **Fechas que no coinciden** con las visibles.

## Notas relacionadas

- [[03 Person y Organization]] — los tipos de `author` y `publisher`.
- [[06 Artículos (article)]] — el marcado HTML que esto complementa.
- [[01 Schema.org Básico]] — la sintaxis JSON-LD.
