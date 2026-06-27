---
title: "twitter:card — Tipo de tarjeta"
aliases:
  - twitter card type
tags:
  - html
  - api/concepto
  - metadatos
draft: false
order: 1
---

# Tipo de Tarjeta (twitter card)

> [!definicion]
> `twitter:card` define **qué tipo de tarjeta** se muestra al compartir el enlace en X. Es la etiqueta Twitter **imprescindible**: sin ella, X no genera tarjeta enriquecida (aunque haya Open Graph). El valor más usado es `summary_large_image`.

```html
<meta name="twitter:card" content="summary_large_image" />
```

## Los tipos de tarjeta

| `twitter:card` | Aspecto |
|----------------|---------|
| `summary` | Tarjeta pequeña: imagen cuadrada a un lado, título y descripción |
| `summary_large_image` | Tarjeta grande: imagen ancha destacada arriba, título debajo |
| `app` | Tarjeta para promocionar una app móvil |
| `player` | Tarjeta con un reproductor de vídeo/audio incrustado |

## summary vs. summary_large_image

> [!info] Cuál elegir
> - **`summary_large_image`**: la opción habitual para artículos, blogs y productos. La imagen grande (1200×630, como Open Graph) capta mucha más atención.
> - **`summary`**: tarjeta compacta con miniatura cuadrada. Útil cuando la imagen no es protagonista o el espacio importa.
>
> Para casi todo el contenido editorial, `summary_large_image` rinde mejor.

## La imagen según el tipo

El tipo de tarjeta condiciona la imagen óptima:

| Tipo | Imagen recomendada |
|------|--------------------|
| `summary` | Cuadrada, mínimo 144×144 (idealmente 1:1) |
| `summary_large_image` | 1.91:1, ~1200×630 (igual que Open Graph) |

## Buenas prácticas

> [!tip] Recomendaciones
> - Incluye `twitter:card` (al menos esta) en todo contenido compartible.
> - Usa `summary_large_image` para artículos y productos.
> - Si reaprovechas `og:image` (1200×630), `summary_large_image` encaja perfecto.
> - Verifica con el Card Validator de X antes de difundir.

## Errores comunes

> [!warning] Trampas
> - **Sin `twitter:card`**: X no genera tarjeta aunque haya Open Graph.
> - **`summary` con imagen ancha**: se recorta a cuadrada y se ve mal.
> - **Usar `property=`** en vez de `name=` para las etiquetas Twitter.

## Notas relacionadas

- [[02 Contenido (title, description, image)]] — el contenido de la tarjeta.
- [[02 Open Graph/index]] — el respaldo del que X toma los datos.
