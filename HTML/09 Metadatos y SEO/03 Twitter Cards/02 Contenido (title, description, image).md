---
title: "twitter:title, description, image — Contenido de la tarjeta"
aliases:
  - twitter content
tags:
  - html
  - api/concepto
  - metadatos
draft: false
order: 2
---

# Contenido (title, description, image)

> [!definicion]
> Las etiquetas `twitter:title`, `twitter:description` y `twitter:image` definen el **contenido** de la tarjeta en X. Solo hace falta declararlas cuando se quiere un contenido **distinto** del de [[02 Open Graph/index | Open Graph]]; si no, X las hereda de las `og:`.

```html
<meta name="twitter:title" content="Tarta de manzana fácil" />
<meta name="twitter:description" content="Receta en 45 minutos con 6 ingredientes." />
<meta name="twitter:image" content="https://sitio.com/tarta.jpg" />
<meta name="twitter:image:alt" content="Tarta de manzana recién horneada" />
```

## Las etiquetas y sus límites

| Etiqueta | Contenido | Límite aprox. |
|----------|-----------|---------------|
| `twitter:title` | Título de la tarjeta | ~70 caracteres |
| `twitter:description` | Descripción | ~200 caracteres |
| `twitter:image` | URL absoluta de la imagen | — |
| `twitter:image:alt` | Texto alternativo de la imagen | ~420 caracteres |

## Herencia de Open Graph

> [!info] Declara solo lo que difiera
> X aplica esta cascada: usa `twitter:title` si existe, si no `og:title`; `twitter:image` o `og:image`; etc. Por eso, con Open Graph completo, **no necesitas repetir** estas etiquetas salvo que quieras un texto o imagen específicos para X (por ejemplo, una imagen con otro recorte o un título más informal para la audiencia de la plataforma).

## twitter:image:alt por accesibilidad

`twitter:image:alt` aporta el texto alternativo de la imagen de la tarjeta, que X usa para usuarios con lector de pantalla. Es el equivalente de `og:image:alt`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Si tienes Open Graph completo, declara solo `twitter:card` y deja que herede.
> - Añade etiquetas `twitter:` específicas solo para contenido distinto en X.
> - Incluye `twitter:image:alt` por accesibilidad.
> - Respeta los límites de longitud para que no se trunque el texto.

## Errores comunes

> [!warning] Trampas
> - **Duplicar todo** innecesariamente cuando Open Graph ya lo cubre.
> - **URL de imagen relativa**: debe ser absoluta.
> - **Texto demasiado largo**: se trunca en la tarjeta.

## Notas relacionadas

- [[01 Tipo de Tarjeta (twitter card)]] — la etiqueta imprescindible.
- [[02 Imagen (og image)]] — la imagen que X reutiliza por defecto.
