---
title: <track> — Subtítulos y pistas de texto
aliases:
  - track element
  - captions subtitles
tags:
  - html
  - api/elemento
  - a11y
elemento: track
categoria: incrustado
rol_implicito: ninguno
vacio: true
draft: false
order: 4
---

# Pistas de Texto (track)

> [!definicion]
> `<track>` añade **pistas de texto temporizado** a un [[01 Video (video) | `<video>`]] o [[02 Audio (audio) | `<audio>`]]: subtítulos, descripciones, capítulos. Es la pieza clave de accesibilidad del vídeo, y se basa en archivos de subtítulos en formato **WebVTT** (`.vtt`).

```html
<video controls>
  <source src="clip.mp4" type="video/mp4" />
  <track kind="captions" src="es.vtt" srclang="es" label="Español" default />
  <track kind="subtitles" src="en.vtt" srclang="en" label="English" />
</video>
```

## Atributos

| Atributo | Descripción |
|----------|-------------|
| `kind` | Tipo de pista (ver abajo) |
| `src` | Ruta del archivo `.vtt` |
| `srclang` | Idioma de la pista (código de idioma) |
| `label` | Nombre visible en el menú de pistas |
| `default` | Pista activada por defecto |

## Tipos de pista (kind)

| `kind` | Para qué |
|--------|----------|
| `captions` | Subtítulos para personas **sordas**: diálogo **+ sonidos** relevantes ("[música tensa]") |
| `subtitles` | Traducción del diálogo a otro idioma (asume que se oye el audio) |
| `descriptions` | Descripción de lo visual, para personas ciegas (la lee el lector) |
| `chapters` | Títulos de capítulos para navegar el vídeo |
| `metadata` | Datos para scripts, no visibles |

> [!info] captions vs. subtitles: la diferencia clave
> No son sinónimos:
> - **`captions`** (subtítulos para sordos): incluyen el diálogo **y** los sonidos importantes ("[teléfono sonando]", "[risas]"), porque el espectador no oye nada.
> - **`subtitles`**: solo traducen el diálogo, asumiendo que el espectador **sí oye** (solo no entiende el idioma).
>
> Para accesibilidad real de personas sordas, se usan `captions`, no `subtitles`.

## El formato WebVTT

El archivo `.vtt` define los textos con sus tiempos:

```
WEBVTT

00:00:01.000 --> 00:00:04.000
Bienvenidos a la presentación.

00:00:04.500 --> 00:00:07.000
[música suave]
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Incluye `captions` para todo vídeo con diálogo (accesibilidad para sordos).
> - Usa `subtitles` para traducciones; `descriptions` para personas ciegas.
> - Marca con `default` la pista que debe activarse de inicio (según el idioma del sitio).
> - Etiqueta cada pista con `label` y `srclang` claros.

## Errores comunes

> [!warning] Trampas
> - **Usar `subtitles` como accesibilidad**: omiten los sonidos no verbales; usa `captions`.
> - **Vídeo sin ninguna pista**: inaccesible para personas sordas.
> - **`.vtt` mal formado** (sin la cabecera `WEBVTT` o tiempos incorrectos): no carga.

## Notas relacionadas

- [[01 Video (video)]] — el contenedor de las pistas.
- [[01 Subtítulos (captions)]] — los subtítulos desde la accesibilidad (A11Y).
- [[06 Multimedia Accesible/index]] — el panorama de medios accesibles.
