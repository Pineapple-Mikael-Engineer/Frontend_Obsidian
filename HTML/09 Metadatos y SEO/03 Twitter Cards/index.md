---
title: Twitter Cards — Tarjetas enriquecidas en X/Twitter
aliases:
  - twitter cards
tags:
  - html
  - api/concepto
  - metadatos
draft: false
order: 3
---

# Twitter Cards

> [!definicion]
> Las **Twitter Cards** (X) son metadatos `<meta name="twitter:...">` que controlan cómo se ve un enlace al compartirlo en X/Twitter: una tarjeta con imagen, título y descripción en lugar de un enlace plano. Son el equivalente de [[02 Open Graph/index | Open Graph]] para esa plataforma.

```html
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="Tarta de manzana fácil" />
<meta name="twitter:description" content="Receta en 45 minutos." />
<meta name="twitter:image" content="https://sitio.com/tarta.jpg" />
```

## Reaprovecha Open Graph

> [!info] Si ya tienes Open Graph, casi está hecho
> X **usa Open Graph como respaldo**: si no encuentra `twitter:title`, usa `og:title`; si no hay `twitter:image`, usa `og:image`, etc. Por eso, con Open Graph bien puesto, a menudo basta añadir **una sola** etiqueta Twitter: el tipo de tarjeta.
> ```html
> <meta name="twitter:card" content="summary_large_image" />
> <!-- el resto lo toma de las og:... -->
> ```
> Las etiquetas `twitter:` específicas solo hacen falta cuando quieres un contenido **distinto** del de Open Graph para X.

## name, no property

A diferencia de Open Graph (que usa `property=`), Twitter Cards usan `name=`, como los `<meta>` normales:

```html
<meta name="twitter:card" content="…" />   <!-- name, no property -->
```

## Mapa de la sección

- [[01 Tipo de Tarjeta (twitter card)]] — los tipos de tarjeta (`summary`, `summary_large_image`…).
- [[02 Contenido (title, description, image)]] — el contenido de la tarjeta.
- [[03 Atribución (site, creator)]] — vincular la tarjeta a cuentas de X.

## Notas relacionadas

- [[02 Open Graph/index]] — el sistema que X reutiliza como respaldo.
- [[01 Tipo de Tarjeta (twitter card)]] — la etiqueta mínima imprescindible.
