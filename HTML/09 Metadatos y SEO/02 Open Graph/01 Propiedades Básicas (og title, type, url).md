---
title: "Open Graph básico — og:title, og:type, og:url, og:image"
aliases:
  - og basic
  - propiedades open graph
tags:
  - html
  - api/concepto
  - metadatos
draft: false
order: 1
---

# Propiedades Básicas (og title, type, url)

> [!definicion]
> Open Graph define **cuatro propiedades obligatorias** para que una página tenga una tarjeta de previsualización válida al compartirse: `og:title`, `og:type`, `og:image` y `og:url`. Con estas cuatro, cualquier red puede construir la tarjeta.

```html
<meta property="og:title" content="Tarta de manzana fácil" />
<meta property="og:type" content="article" />
<meta property="og:image" content="https://sitio.com/tarta.jpg" />
<meta property="og:url" content="https://sitio.com/tarta" />
```

## Las cuatro obligatorias

| Propiedad | Qué es |
|-----------|--------|
| `og:title` | El título de la tarjeta (puede diferir del `<title>`, sin el nombre del sitio) |
| `og:type` | El tipo de objeto: `website`, `article`, `product`, `video.movie`… |
| `og:image` | La imagen de previsualización (ver nota dedicada) |
| `og:url` | La URL canónica del contenido |

## og:type y propiedades adicionales

El `og:type` determina qué **propiedades extra** tienen sentido:

| `og:type` | Propiedades adicionales típicas |
|-----------|--------------------------------|
| `website` | (ninguna específica) |
| `article` | `article:published_time`, `article:author`, `article:section` |
| `product` | `product:price:amount`, `product:price:currency` |
| `profile` | `profile:first_name`, `profile:last_name` |

```html
<meta property="og:type" content="article" />
<meta property="article:published_time" content="2026-06-04T10:00:00Z" />
<meta property="article:author" content="Ana López" />
```

## og:title vs. title

> [!info] Pueden (y suelen) diferir
> El `<title>` se optimiza para Google e incluye el nombre del sitio ("Tarta — Recetas de la Abuela"). El `og:title` se optimiza para el **clic social** y suele ser más directo, sin la marca ("Tarta de manzana fácil en 45 minutos"). No tienen por qué coincidir: cada uno sirve a un canal distinto.

## og:url y la canónica

`og:url` debería apuntar a la **URL canónica** del contenido (coherente con [[11 Enlace Canónico (link rel canonical) | rel=canonical]]). Así, los "me gusta" y veces compartido se consolidan en una sola URL, aunque la página se comparta desde variantes con parámetros.

## Buenas prácticas

> [!tip] Recomendaciones
> - Incluye **siempre** las cuatro básicas en contenido compartible.
> - Usa `property=`, no `name=`.
> - Adapta `og:type` al contenido y añade sus propiedades específicas.
> - Haz coincidir `og:url` con la URL canónica.
> - Optimiza `og:title` para el clic, no necesariamente igual al `<title>`.

## Errores comunes

> [!warning] Trampas
> - **`name=` en vez de `property=`**: las etiquetas se ignoran.
> - **Faltar `og:image`**: la tarjeta sale sin imagen, mucho menos atractiva.
> - **`og:url` con parámetros de tracking**: fragmenta las métricas de compartido.

## Notas relacionadas

- [[02 Imagen (og image)]] — la imagen de la tarjeta y sus requisitos.
- [[03 Sitio y Locale (og site_name, locale)]] — propiedades complementarias.
- [[11 Enlace Canónico (link rel canonical)]] — la coherencia de `og:url`.
