---
title: Multimedia Accesible — Alternativas para vídeo y audio
aliases:
  - accessible media
tags:
  - html
  - api/concepto
  - a11y
draft: false
order: 4
---

# Multimedia Accesible

> [!definicion]
> El vídeo y el audio excluyen a quien no puede oírlos o verlos, salvo que se ofrezcan **alternativas**: [[01 Subtítulos (captions) | subtítulos]] para personas sordas, [[02 Transcripciones | transcripciones]] del contenido hablado, y [[03 Audiodescripción | audiodescripción]] de lo visual para personas ciegas. Son requisitos de las WCAG para todo contenido multimedia.

```html
<video controls>
  <source src="clip.mp4" type="video/mp4" />
  <track kind="captions" src="es.vtt" srclang="es" label="Español" default />
</video>
```

## Qué necesita cada medio

| Medio | Para personas sordas | Para personas ciegas |
|-------|----------------------|----------------------|
| Vídeo | Subtítulos (captions) | Audiodescripción |
| Audio (podcast) | Transcripción | (ya es audio) |
| Vídeo sin diálogo | (no aplica) | Audiodescripción o transcripción |

## Mapa de la sección

- [[01 Subtítulos (captions)]] — el diálogo y los sonidos relevantes como texto sincronizado.
- [[02 Transcripciones]] — todo el contenido en texto, para audio y vídeo.
- [[03 Audiodescripción]] — narrar lo visual para personas ciegas.

## La base técnica: track y WebVTT

Los subtítulos y descripciones temporizadas se añaden con el elemento [[04 Pistas de Texto (track) | `<track>`]] y archivos **WebVTT** (`.vtt`). Es el mecanismo HTML que materializa estas alternativas; esta sección cubre el **porqué** y el **qué escribir**, mientras que `<track>` cubre el **cómo** técnico.

## No es opcional

> [!info] Requisito legal y de norma
> Las alternativas multimedia no son un extra: son criterios **WCAG de nivel A/AA** (1.2.x), y en muchos países un **requisito legal** para sitios públicos y empresas. Un vídeo sin subtítulos excluye a millones de personas sordas; un podcast sin transcripción, a las personas sordas y a quien prefiere leer. Planificarlas desde la producción es mucho más barato que añadirlas después.

## Notas relacionadas

- [[01 Subtítulos (captions)]] — el caso más común y exigido.
- [[04 Pistas de Texto (track)]] — el elemento `<track>` y WebVTT.
- [[02 Video y Audio/index]] — los elementos multimedia.
