---
title: Video y Audio — Medios reproducibles
aliases:
  - media elements
  - video audio
tags:
  - html
  - api/concepto
  - multimedia
draft: false
---

# Video y Audio

> [!definicion]
> HTML reproduce medios de forma nativa, sin plugins, con [[01 Video (video) | `<video>`]] y [[02 Audio (audio) | `<audio>`]]. Ambos admiten varias [[03 Fuentes (source) | `<source>`]] (distintos formatos como respaldo) y [[04 Pistas de Texto (track) | `<track>`]] para subtítulos y descripciones.

```html
<video controls width="640">
  <source src="clip.webm" type="video/webm" />
  <source src="clip.mp4" type="video/mp4" />
  <track kind="captions" src="subs.vtt" srclang="es" label="Español" default />
  Tu navegador no soporta vídeo.
</video>
```

## Mapa de la sección

- [[01 Video (video)]] — el reproductor de vídeo y sus atributos.
- [[02 Audio (audio)]] — el reproductor de audio.
- [[03 Fuentes (source)]] — ofrecer varios formatos para compatibilidad.
- [[04 Pistas de Texto (track)]] — subtítulos, descripciones y capítulos.

## Atributos comunes a video y audio

`<video>` y `<audio>` comparten casi todos sus atributos de control:

| Atributo | Efecto |
|----------|--------|
| `controls` | Muestra los controles nativos (play, volumen…) |
| `autoplay` | Reproduce al cargar (con restricciones; requiere `muted` en vídeo) |
| `muted` | Empieza silenciado |
| `loop` | Repite al terminar |
| `preload` | `none`, `metadata`, `auto`: cuánto precargar |

## Accesibilidad: el hilo conductor

> [!info] Medios accesibles
> El vídeo y el audio necesitan alternativas para quien no puede oírlos o verlos:
> - **Subtítulos** (`<track kind="captions">`) para personas sordas.
> - **Transcripción** textual para audio.
> - **Audiodescripción** para personas ciegas (lo visual narrado).
>
> El detalle de estas alternativas está en [[06 Multimedia Accesible/index | Multimedia accesible]] (A11Y).

## Notas relacionadas

- [[01 Video (video)]] — el caso más completo.
- [[04 Pistas de Texto (track)]] — los subtítulos.
- [[06 Multimedia Accesible/index]] — las alternativas de accesibilidad.
