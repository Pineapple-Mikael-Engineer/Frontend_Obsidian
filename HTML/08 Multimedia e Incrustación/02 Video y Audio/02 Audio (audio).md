---
title: <audio> — Reproductor de audio
aliases:
  - audio element
tags:
  - html
  - api/elemento
  - multimedia
elemento: audio
categoria: incrustado
rol_implicito: ninguno
vacio: false
draft: false
---

# Audio (audio)

> [!definicion]
> `<audio>` reproduce sonido de forma nativa: música, podcasts, efectos. Funciona igual que [[01 Video (video) | `<video>`]] pero sin imagen. Acepta varias [[03 Fuentes (source) | `<source>`]] para compatibilidad de formatos.

```html
<audio controls>
  <source src="podcast.ogg" type="audio/ogg" />
  <source src="podcast.mp3" type="audio/mpeg" />
  Tu navegador no soporta audio.
</audio>
```

## Atributos

Comparte casi todos con `<video>`:

| Atributo | Efecto |
|----------|--------|
| `controls` | Muestra los controles nativos (play, volumen, barra) |
| `autoplay` | Reproduce al cargar (bloqueado con sonido sin interacción) |
| `muted` | Silenciado de inicio |
| `loop` | Repite al terminar |
| `preload` | `none` / `metadata` / `auto` |

## Sin controls no se ve nada

> [!warning] audio sin controls es invisible
> A diferencia del vídeo (que ocupa espacio con su imagen), un `<audio>` **sin `controls`** no muestra **nada** en pantalla: no hay interfaz. Solo tiene sentido omitir `controls` si vas a controlar la reproducción enteramente con JavaScript (efectos de sonido, música de fondo de un juego). Para que el usuario pueda reproducir, pon `controls`.

## Formatos de audio

| Formato | `type` | Notas |
|---------|--------|-------|
| MP3 | `audio/mpeg` | Soporte universal |
| OGG (Vorbis) | `audio/ogg` | Libre; buen soporte salvo Safari |
| AAC / M4A | `audio/aac` | Buena calidad, amplio soporte |
| WAV | `audio/wav` | Sin pérdida pero muy pesado |

Ofrecer MP3 como respaldo garantiza reproducción casi universal.

## Accesibilidad: la transcripción

> [!info] El audio necesita transcripción
> Para personas sordas, el contenido hablado de un audio (un podcast, una entrevista) debe ofrecerse también como **transcripción textual** cerca del reproductor. `<audio>` no tiene subtítulos como el vídeo; la alternativa es el texto. Detalle en [[02 Transcripciones | Transcripciones]] (A11Y).

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon `controls` salvo que controles la reproducción con JS.
> - Ofrece MP3 como respaldo junto a un formato libre (OGG).
> - Acompaña el audio hablado de una transcripción.
> - `autoplay` con sonido está bloqueado: no lo uses para música no solicitada.

## Errores comunes

> [!warning] Trampas
> - **Sin `controls`** esperando que se vea algo: es invisible.
> - **Un solo formato** sin respaldo MP3.
> - **Audio hablado sin transcripción**: inaccesible para personas sordas.

## Notas relacionadas

- [[01 Video (video)]] — el equivalente con imagen.
- [[03 Fuentes (source)]] — ofrecer varios formatos.
- [[02 Transcripciones]] — la alternativa textual del audio.
