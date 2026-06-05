---
title: <video> — Reproductor de vídeo
aliases:
  - video element
tags:
  - html
  - api/elemento
  - multimedia
elemento: video
categoria: incrustado
rol_implicito: ninguno
vacio: false
draft: false
---

# Video (video)

> [!definicion]
> `<video>` reproduce vídeo de forma nativa en el navegador, sin plugins. Acepta el vídeo directamente con `src` o, mejor, varias [[03 Fuentes (source) | `<source>`]] de distinto formato, más [[04 Pistas de Texto (track) | `<track>`]] para subtítulos.

```html
<video controls width="640" poster="portada.jpg">
  <source src="clip.webm" type="video/webm" />
  <source src="clip.mp4" type="video/mp4" />
  <track kind="captions" src="subs.vtt" srclang="es" label="Español" default />
  Tu navegador no soporta vídeo.
</video>
```

## Atributos

| Atributo | Efecto |
|----------|--------|
| `controls` | Muestra los controles nativos de reproducción |
| `poster` | Imagen de portada antes de reproducir |
| `width` / `height` | Dimensiones (reservan espacio) |
| `autoplay` | Reproduce solo al cargar (requiere `muted`) |
| `muted` | Empieza sin sonido |
| `loop` | Reinicia al terminar |
| `preload` | `none` / `metadata` / `auto` |
| `playsinline` | En móvil, reproduce en línea en vez de a pantalla completa |

## La regla de oro del autoplay

> [!warning] autoplay con sonido está bloqueado
> Los navegadores **bloquean** la reproducción automática con sonido (era una plaga de anuncios molestos). Para que `autoplay` funcione, el vídeo debe ir **silenciado**:
> ```html
> <video autoplay muted loop playsinline>…</video>
> ```
> Esta combinación (`autoplay muted loop playsinline`) es el patrón de los vídeos de fondo decorativos. Con sonido, el navegador exige una interacción del usuario antes de reproducir.

## poster y preload

- **`poster`**: la imagen que se ve antes de darle al play. Sin ella, se muestra el primer fotograma (a veces negro).
- **`preload="none"`**: no descarga nada hasta que el usuario pulsa play (ahorra datos); `metadata` baja solo duración/dimensiones; `auto` deja decidir al navegador.

## Control con JavaScript

`<video>` expone una API rica (`play()`, `pause()`, `currentTime`, eventos `timeupdate`…) para construir reproductores a medida. El detalle vive en el curso de JavaScript.

## Buenas prácticas

> [!tip] Recomendaciones
> - Ofrece varios `<source>` (WebM + MP4) para máxima compatibilidad.
> - Añade `poster` y dimensiones para evitar saltos y mejorar la percepción.
> - Para vídeo de fondo: `autoplay muted loop playsinline`.
> - Incluye `<track>` de subtítulos por accesibilidad.
> - `preload="none"`/`metadata` para no malgastar datos.

## Errores comunes

> [!warning] Trampas
> - **`autoplay` sin `muted`**: el navegador lo bloquea.
> - **Sin subtítulos**: inaccesible para personas sordas.
> - **Sin `playsinline`** en móvil: el vídeo de fondo salta a pantalla completa en iOS.
> - **Un solo formato** sin respaldo: puede no reproducirse en algún navegador.

## Notas relacionadas

- [[03 Fuentes (source)]] — ofrecer varios formatos.
- [[04 Pistas de Texto (track)]] — subtítulos y descripciones.
- [[02 Audio (audio)]] — el equivalente solo de sonido.
