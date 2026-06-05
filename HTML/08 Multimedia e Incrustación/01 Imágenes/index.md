---
title: Imágenes — Insertar mapas de bits en el documento
aliases:
  - images
  - imágenes html
tags:
  - html
  - api/concepto
  - multimedia
draft: false
---

# Imágenes

> [!definicion]
> Las imágenes se insertan con [[01 Imagen (img) | `<img>`]], el elemento básico, ampliado por [[02 Imágenes Responsivas (picture, source) | `<picture>`]] para servir distintas versiones según el dispositivo, [[03 Mapa de Imagen (map, area) | mapas de imagen]] para zonas clicables, y la elección del [[04 Formatos de Imagen | formato]] adecuado (JPEG, PNG, WebP, AVIF, SVG).

```html
<img src="foto.jpg" alt="Atardecer sobre el mar" width="800" height="600" loading="lazy" />
```

## Mapa de la sección

- [[01 Imagen (img)]] — el elemento `<img>` y todos sus atributos (`src`, `alt`, `srcset`, `loading`…).
- [[02 Imágenes Responsivas (picture, source)]] — distintas imágenes según pantalla o formato.
- [[03 Mapa de Imagen (map, area)]] — regiones clicables dentro de una imagen.
- [[04 Formatos de Imagen]] — cuándo JPEG, PNG, GIF, SVG, WebP o AVIF.

## Los dos ejes de una buena imagen

| Eje | Cómo se cuida |
|-----|---------------|
| **Accesibilidad** | El atributo `alt` describe la imagen para quien no la ve |
| **Rendimiento** | Formato adecuado, `srcset`, `loading="lazy"`, dimensiones explícitas |

Toda imagen de contenido debe atender ambos: un `alt` correcto y un peso optimizado. El detalle de cada uno vive en [[01 Imagen (img) | la nota de `<img>`]].

## Mapa de bits vs. vectorial

> [!info] Dos familias de imagen
> - **Mapa de bits (raster)**: píxeles. JPEG, PNG, WebP, AVIF, GIF. Ideales para fotos; pierden nitidez al ampliar.
> - **Vectorial**: fórmulas geométricas. [[02 Gráficos Vectoriales (svg) | SVG]]. Ideal para logos e iconos; escala sin perder calidad.
>
> Esta sección cubre sobre todo los mapas de bits con `<img>`; el SVG, por su naturaleza de código, se trata en [[04 Gráficos/index | Gráficos]].

## Notas relacionadas

- [[01 Imagen (img)]] — el punto de partida.
- [[02 Gráficos Vectoriales (svg)]] — la alternativa vectorial.
- [[11 Figura (figure, figcaption)]] — envolver imágenes con leyenda.
