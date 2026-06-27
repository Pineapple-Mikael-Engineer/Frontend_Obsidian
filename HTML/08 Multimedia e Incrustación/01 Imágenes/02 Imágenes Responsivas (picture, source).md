---
title: <picture> y srcset — Imágenes responsivas
aliases:
  - picture element
  - responsive images
tags:
  - html
  - api/elemento
  - responsive
elemento: picture
categoria: incrustado
rol_implicito: ninguno
vacio: false
draft: false
order: 2
---

# Imágenes Responsivas (picture, source)

> [!definicion]
> Las imágenes responsivas sirven **distintas versiones** de una imagen según el dispositivo: una pequeña en móvil, una grande en escritorio, o un formato moderno con respaldo. Hay dos mecanismos: `srcset`/`sizes` en el propio [[01 Imagen (img) | `<img>`]] (resolución) y el elemento `<picture>` con `<source>` (control total).

```html
<picture>
  <source srcset="foto.avif" type="image/avif" />
  <source srcset="foto.webp" type="image/webp" />
  <img src="foto.jpg" alt="Descripción" />
</picture>
```

## Dos problemas, dos herramientas

| Problema | Solución |
|----------|----------|
| Misma imagen, distinta **resolución** según pantalla | `srcset` + `sizes` en `<img>` |
| Distinta imagen según **arte/formato/media** | `<picture>` + `<source>` |

### srcset por resolución (resolution switching)

El navegador elige el archivo según el ancho real y la densidad de píxeles, sin cambiar el contenido de la imagen:

```html
<img src="foto-800.jpg"
     srcset="foto-400.jpg 400w, foto-800.jpg 800w, foto-1600.jpg 1600w"
     sizes="(max-width: 600px) 100vw, 800px"
     alt="…" />
```

- `srcset` lista los archivos con su ancho real (`400w`).
- `sizes` indica cuánto **ocupará** la imagen en cada caso, para que el navegador calcule qué archivo bajar.

### picture para arte y formato

`<picture>` da control explícito: el navegador usa el **primer `<source>` que coincida**, y cae al `<img>` final si ninguno aplica.

```html
<picture>
  <!-- Distinto recorte en móvil (art direction) -->
  <source media="(max-width: 600px)" srcset="retrato.jpg" />
  <source media="(min-width: 601px)" srcset="panoramica.jpg" />
  <img src="panoramica.jpg" alt="…" />
</picture>
```

## El img final es obligatorio

> [!warning] picture sin img no muestra nada
> El `<img>` dentro de `<picture>` **no es opcional**: es el que realmente se muestra y el que lleva el `alt`. Los `<source>` solo proponen alternativas; si ninguna aplica (o el navegador es antiguo), se usa el `<img>`. Un `<picture>` sin `<img>` no renderiza imagen. El `alt` va siempre en el `<img>`, no en los `<source>`.

## Casos de uso

| Caso | Mecanismo |
|------|-----------|
| Servir AVIF/WebP con respaldo JPEG | `<picture>` + `<source type>` |
| Distinto recorte en móvil vs. escritorio | `<picture>` + `<source media>` |
| Misma foto a varias resoluciones | `<img srcset sizes>` |
| Pantallas retina (2x) | `srcset` con descriptores `2x` |

## Buenas prácticas

> [!tip] Recomendaciones
> - Para solo cambiar resolución, `srcset`/`sizes` en `<img>` (más simple).
> - Para cambiar formato o recorte, `<picture>` con `<source>`.
> - Ordena los `<source>` del más deseable al menos (el navegador toma el primero que aplica).
> - El `alt` y el `src` de respaldo van en el `<img>` final, siempre presente.

## Errores comunes

> [!warning] Trampas
> - **`<picture>` sin `<img>`**: no muestra nada.
> - **`alt` en `<source>`**: el `alt` va en el `<img>`.
> - **`sizes` incoherente** con el layout real: el navegador baja el archivo equivocado.

## Notas relacionadas

- [[01 Imagen (img)]] — el elemento base y `srcset`/`sizes`.
- [[04 Formatos de Imagen]] — qué formatos servir con `<source type>`.
- [[02 Viewport (meta viewport)]] — el prerrequisito del diseño responsivo.
