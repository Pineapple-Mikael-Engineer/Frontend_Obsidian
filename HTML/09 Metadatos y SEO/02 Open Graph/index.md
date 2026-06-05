---
title: Open Graph — Cómo se ve la página al compartirla
aliases:
  - open graph
  - og tags
tags:
  - html
  - api/concepto
  - metadatos
draft: false
---

# Open Graph

> [!definicion]
> El **protocolo Open Graph** (creado por Facebook) define metadatos `<meta property="og:...">` que controlan cómo se ve la página cuando se **comparte** en redes sociales y apps de mensajería: el título, la imagen, la descripción de la tarjeta de previsualización. Lo usan Facebook, LinkedIn, WhatsApp, Telegram, Slack, Discord y muchos más.

```html
<meta property="og:title" content="Tarta de manzana fácil" />
<meta property="og:description" content="Receta en 45 minutos con 6 ingredientes." />
<meta property="og:image" content="https://sitio.com/tarta.jpg" />
<meta property="og:url" content="https://sitio.com/tarta" />
<meta property="og:type" content="article" />
```

## Por qué importa

> [!info] La tarjeta vende el clic
> Sin Open Graph, al compartir un enlace la red social improvisa una previsualización (a veces fea o vacía: sin imagen, con un título genérico). Con Open Graph, **controlas** exactamente qué imagen, título y texto aparecen, lo que dispara los clics. Para cualquier contenido que se vaya a compartir (artículos, productos), es tan importante como la `meta description` para Google.

## property, no name

> [!warning] Open Graph usa property, no name
> A diferencia de los `<meta name="...">` habituales, Open Graph usa `<meta property="og:...">`. Es un detalle fácil de olvidar que hace que las etiquetas no funcionen:
> ```html
> <meta property="og:title" content="…" />   <!-- ✅ -->
> <meta name="og:title" content="…" />        <!-- ❌ no lo reconoce -->
> ```

## Mapa de la sección

- [[01 Propiedades Básicas (og title, type, url)]] — las 4 propiedades obligatorias.
- [[02 Imagen (og image)]] — la imagen de la tarjeta y sus dimensiones.
- [[03 Sitio y Locale (og site_name, locale)]] — nombre del sitio e idioma.

## Validar antes de compartir

Cada red ofrece un depurador para previsualizar y refrescar la caché de la tarjeta (Facebook Sharing Debugger, LinkedIn Post Inspector). Conviene revisarlo, porque las redes **cachean** los metadatos y no reflejan cambios al instante.

## Notas relacionadas

- [[01 Propiedades Básicas (og title, type, url)]] — el punto de partida.
- [[03 Twitter Cards/index]] — el equivalente para X/Twitter, que reaprovecha Open Graph.
- [[04 Descripción (meta description)]] — el equivalente para los resultados de Google.
