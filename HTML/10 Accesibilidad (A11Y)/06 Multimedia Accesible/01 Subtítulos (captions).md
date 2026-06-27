---
title: Subtítulos (captions) — Diálogo y sonido como texto
aliases:
  - captions
  - subtítulos
tags:
  - html
  - api/concepto
  - a11y
draft: false
order: 1
---

# Subtítulos (captions)

> [!definicion]
> Los **subtítulos para personas sordas** (captions) muestran, sincronizado con el vídeo, **todo lo que se oye**: el diálogo y los **sonidos relevantes** ("[teléfono sonando]", "[música tensa]", "[risas]"). Son la alternativa accesible principal del vídeo, y se añaden con [[04 Pistas de Texto (track) | `<track kind="captions">`]].

```html
<video controls>
  <source src="clip.mp4" type="video/mp4" />
  <track kind="captions" src="es.vtt" srclang="es" label="Español" default />
</video>
```

## captions vs. subtitles

> [!info] No son lo mismo
> | | `captions` | `subtitles` |
> |--|-----------|-------------|
> | Incluyen el diálogo | Sí | Sí |
> | Incluyen sonidos no verbales | **Sí** ("[aplausos]") | No |
> | Asumen que el espectador oye | **No** (es sordo) | Sí (solo no entiende el idioma) |
> | Función | Accesibilidad | Traducción |
>
> Para accesibilidad de personas sordas se usan **captions**, no subtitles: omitir los sonidos relevantes ("[explosión]", "[suspira]") deja a la persona sorda sin información que sí recibe quien oye.

## Qué incluir en los captions

- **Diálogo**, identificando quién habla si no es evidente.
- **Sonidos relevantes** entre corchetes: `[música suave]`, `[golpe seco]`, `[teléfono]`.
- **Tono o intención** cuando aporta: `[con sarcasmo]`.
- **Música**: el título/letra si es relevante (`♪ Canción de cumpleaños ♪`).

## Subtítulos abiertos vs. cerrados

| Tipo | Cómo |
|------|------|
| **Cerrados** (closed) | El usuario los activa/desactiva (`<track>` + `.vtt`) — recomendado |
| **Abiertos** (open) | "Quemados" en el vídeo, siempre visibles, no desactivables |

Los cerrados son preferibles: el usuario decide, y el texto es seleccionable y escalable. Los abiertos solo cuando la plataforma no admite pistas.

## Calidad: revisar los automáticos

> [!warning] Los subtítulos automáticos no bastan
> Las transcripciones automáticas (de YouTube, IA) son un punto de partida, pero contienen errores —nombres propios, términos técnicos, puntuación— y rara vez incluyen los sonidos no verbales. Para cumplir accesibilidad, deben **revisarse y corregirse**. Unos captions llenos de errores pueden ser tan confusos como no tenerlos.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `kind="captions"` (no `subtitles`) para accesibilidad.
> - Incluye los sonidos relevantes, no solo el diálogo.
> - Subtítulos cerrados (activables) mejor que quemados.
> - Revisa y corrige los subtítulos automáticos antes de publicar.
> - Sincroniza bien los tiempos en el `.vtt`.

## Errores comunes

> [!warning] Trampas
> - **Usar `subtitles` como accesibilidad**: faltan los sonidos no verbales.
> - **Publicar autogenerados sin revisar**: errores y omisiones.
> - **Solo diálogo**, sin describir música ni efectos relevantes.

## Notas relacionadas

- [[04 Pistas de Texto (track)]] — el elemento `<track>` y el formato WebVTT.
- [[02 Transcripciones]] — la alternativa textual completa.
- [[01 Video (video)]] — el reproductor que aloja los subtítulos.
