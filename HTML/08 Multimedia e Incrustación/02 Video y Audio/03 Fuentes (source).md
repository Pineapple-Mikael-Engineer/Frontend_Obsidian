---
title: <source> — Fuentes alternativas de medios
aliases:
  - source element
tags:
  - html
  - api/elemento
  - multimedia
elemento: source
categoria: incrustado
rol_implicito: ninguno
vacio: true
draft: false
---

# Fuentes (source)

> [!definicion]
> `<source>` ofrece una **fuente alternativa** dentro de [[01 Video (video) | `<video>`]], [[02 Audio (audio) | `<audio>`]] o [[02 Imágenes Responsivas (picture, source) | `<picture>`]]. El navegador prueba las fuentes en orden y usa la **primera que pueda reproducir**, lo que garantiza compatibilidad entre formatos.

```html
<video controls>
  <source src="clip.webm" type="video/webm" />
  <source src="clip.mp4" type="video/mp4" />
  Texto de respaldo si nada funciona.
</video>
```

## Por qué varias fuentes

> [!info] No todos los navegadores soportan los mismos formatos
> El soporte de formatos de vídeo/audio varía entre navegadores (por licencias y códecs). Ofrecer varios `<source>` deja que cada navegador elija el que entiende: WebM (libre, eficiente) con MP4 (universal) como respaldo cubre prácticamente todos los casos. El navegador **no descarga** las fuentes que descarta: solo baja la elegida.

## Atributos

| Atributo | Descripción |
|----------|-------------|
| `src` | Ruta del archivo (en video/audio) |
| `type` | Tipo MIME del recurso (`video/webm`, `audio/mpeg`) |
| `srcset` | En `<picture>`: el conjunto de imágenes |
| `media` | En `<picture>`: condición de media query |

## El atributo type acelera la elección

Incluir `type` permite al navegador **saber si puede reproducir** la fuente **sin descargarla**: descarta las que no soporta por el MIME y va directo a la compatible. Sin `type`, tendría que intentar cada una:

```html
<source src="clip.webm" type="video/webm" />   <!-- el navegador sabe si lo soporta -->
```

## El orden importa

El navegador toma la **primera** fuente compatible, así que se ordenan de la más deseable (más eficiente, mejor calidad) a la de respaldo más compatible:

```html
<source src="clip.av1.mp4" type="video/mp4; codecs=av01…" />  <!-- preferida -->
<source src="clip.webm" type="video/webm" />
<source src="clip.h264.mp4" type="video/mp4" />               <!-- respaldo universal -->
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Incluye siempre el atributo `type` para que el navegador elija sin descargar de más.
> - Ordena de la fuente más eficiente a la más compatible.
> - Para vídeo: WebM + MP4. Para audio: OGG + MP3.
> - Pon texto de respaldo dentro del `<video>`/`<audio>` para navegadores sin soporte.

## Errores comunes

> [!warning] Trampas
> - **Omitir `type`**: el navegador puede descargar fuentes que no soporta.
> - **Una sola fuente** sin respaldo universal (MP4/MP3).
> - **Confundir el `<source>` de media** (`<picture>`) con el de medios (`<video>`): atributos distintos (`srcset`/`media` vs. `src`/`type`).

## Notas relacionadas

- [[01 Video (video)]] · [[02 Audio (audio)]] — los contenedores de las fuentes.
- [[02 Imágenes Responsivas (picture, source)]] — `<source>` en imágenes responsivas.
