---
title: "og:image — La imagen de la tarjeta social"
aliases:
  - og image
tags:
  - html
  - api/concepto
  - metadatos
draft: false
order: 2
---

# Imagen (og image)

> [!definicion]
> `og:image` define la **imagen de previsualización** que aparece al compartir la página. Es, con diferencia, lo que más capta la atención en una tarjeta social: una buena imagen multiplica los clics. Conviene cuidar sus dimensiones y formato.

```html
<meta property="og:image" content="https://sitio.com/og/tarta.jpg" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
<meta property="og:image:alt" content="Tarta de manzana recién horneada" />
```

## Dimensiones recomendadas

> [!info] 1200 × 630 es el estándar
> La proporción recomendada es **1.91:1**, típicamente **1200 × 630 px**. Esta medida se ve bien en Facebook, LinkedIn, Twitter (con tarjeta grande) y la mayoría de plataformas. Imágenes más pequeñas pueden mostrarse como miniatura cuadrada o recortarse; declarar `og:image:width`/`height` ayuda a la red a renderizar la tarjeta sin esperar a descargar la imagen.

| Propiedad | Valor |
|-----------|-------|
| `og:image` | URL **absoluta** de la imagen |
| `og:image:width` | `1200` |
| `og:image:height` | `630` |
| `og:image:alt` | Texto alternativo de la imagen |
| `og:image:type` | `image/jpeg`, `image/png` |

## La URL debe ser absoluta

> [!warning] og:image necesita URL completa
> A diferencia de un `<img src>` en la página (que puede ser relativo), `og:image` **debe ser una URL absoluta** (`https://dominio.com/imagen.jpg`). Las redes rastrean la página desde fuera y no resuelven rutas relativas. Una ruta relativa deja la tarjeta sin imagen.

## Buenas prácticas

> [!tip] Recomendaciones
> - Imagen de **1200 × 630 px**, con el contenido importante centrado (los bordes se recortan en algunas vistas).
> - URL **absoluta** y accesible públicamente (sin login).
> - Declara `width`/`height` para un render más rápido y `og:image:alt` por accesibilidad.
> - Peso razonable (las redes la descargan): JPEG optimizado.
> - Diseña imágenes OG específicas (con título superpuesto) para artículos clave.

## Errores comunes

> [!warning] Trampas
> - **URL relativa**: la tarjeta sale sin imagen.
> - **Imagen demasiado pequeña** o con proporción rara: se recorta o no se muestra.
> - **Imagen tras login**: la red no puede acceder a ella.
> - **No refrescar la caché** de la red tras cambiar la imagen (usa el depurador de cada plataforma).

## Notas relacionadas

- [[01 Propiedades Básicas (og title, type, url)]] — las propiedades obligatorias que la acompañan.
- [[03 Twitter Cards/index]] — reutiliza `og:image` para sus tarjetas.
