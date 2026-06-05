---
title: Metadatos y SEO — Información para máquinas y buscadores
aliases:
  - seo
  - metadatos
tags:
  - html
  - api/concepto
  - metadatos
draft: false
---

# Metadatos y SEO

> [!definicion]
> Los metadatos describen la página para **máquinas**: buscadores, redes sociales, agregadores. Bien usados, mejoran cómo aparece la página en Google (SEO) y al compartirla en redes (tarjetas enriquecidas). Viven sobre todo en el [[03 Cabecera (head)/index | `<head>`]].

```html
<head>
  <title>Tarta de manzana — Recetas</title>
  <meta name="description" content="Receta paso a paso…" />
  <meta property="og:title" content="Tarta de manzana" />
  <script type="application/ld+json">{ "@type": "Recipe", … }</script>
</head>
```

## Mapa de la sección

- [[01 Metadatos Estándar/index]] — idioma, robots, verificación, redirecciones.
- [[02 Open Graph/index]] — cómo se ve la página al compartirla (Facebook, LinkedIn, WhatsApp…).
- [[03 Twitter Cards/index]] — tarjetas enriquecidas en X/Twitter.
- [[04 Datos Estructurados (JSON-LD)/index]] — describir el contenido con Schema.org para resultados enriquecidos.

## Las capas del SEO técnico

| Capa | Mecanismo | Efecto |
|------|-----------|--------|
| Básica | `<title>`, `meta description`, encabezados | Cómo aparece en los resultados |
| Indexación | `meta robots`, `canonical`, sitemap | Qué se indexa y cuál es la versión oficial |
| Compartido social | Open Graph, Twitter Cards | Cómo se ve al compartir en redes |
| Enriquecido | JSON-LD (Schema.org) | Estrellas, precios, FAQ en los resultados |

## SEO no es solo metadatos

> [!info] El contenido manda
> Los metadatos ayudan, pero el factor decisivo de SEO es el **contenido real**: relevante, bien estructurado con [[01 Encabezados Jerárquicos (h1-h6) | encabezados]] semánticos, rápido y accesible. Los metadatos describen y presentan ese contenido; no lo sustituyen. Una página vacía con metadatos perfectos no posiciona.

## Los básicos ya vistos

Algunos metadatos clave se tratan en la sección de estructura, junto al `<head>`:

- [[03 Título del Documento (title)]] — el factor on-page más fuerte.
- [[04 Descripción (meta description)]] — el texto del resultado de búsqueda.
- [[11 Enlace Canónico (link rel canonical)]] — la URL oficial.

## Notas relacionadas

- [[02 Open Graph/index]] — el compartido social, lo más visible tras los básicos.
- [[04 Datos Estructurados (JSON-LD)/index]] — los resultados enriquecidos.
