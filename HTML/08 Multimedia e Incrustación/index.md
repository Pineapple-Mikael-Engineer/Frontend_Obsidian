---
title: Multimedia e Incrustación — Imágenes, vídeo, audio y contenido externo
aliases:
  - multimedia
  - embedded content
tags:
  - html
  - api/concepto
  - multimedia
draft: false
---

# Multimedia e Incrustación

> [!definicion]
> Esta sección cubre los elementos que insertan **contenido no textual**: imágenes, vídeo, audio, gráficos y contenido externo (otras páginas, objetos). Son los elementos "incrustados" (embedded content): traen al documento recursos que viven fuera del flujo de texto.

```html
<img src="foto.jpg" alt="Descripción" />
<video src="clip.mp4" controls></video>
<iframe src="https://ejemplo.com" title="Ejemplo"></iframe>
```

## Mapa de la sección

- [[01 Imágenes/index]] — `<img>`, imágenes responsivas, mapas de imagen y formatos.
- [[02 Video y Audio/index]] — `<video>`, `<audio>`, fuentes y pistas de texto.
- [[03 Incrustación/index]] — `<iframe>`, `<object>`, `<embed>` para contenido externo.
- [[04 Gráficos/index]] — `<canvas>` y `<svg>` para dibujo y vectores.

## El hilo común: el texto alternativo

> [!info] Lo no textual necesita una alternativa textual
> El contenido multimedia tiene un requisito transversal de accesibilidad: ofrecer una **alternativa textual** para quien no puede percibirlo. Cada tipo tiene su mecanismo:
> - Imágenes → el atributo `alt`.
> - Vídeo → subtítulos (`<track>`) y transcripciones.
> - Audio → transcripciones.
> - Gráficos → texto accesible (`<canvas>` con contenido de respaldo, `<svg>` con `<title>`).
>
> Es el tema que recorre toda la sección, desarrollado a fondo en [[10 Accesibilidad (A11Y)/index | Accesibilidad]].

## Rendimiento: el peso de los medios

Las imágenes y vídeos son, con diferencia, lo que más pesa en una página. Por eso aparecen aquí técnicas de optimización: formatos modernos (WebP, AVIF), carga diferida (`loading="lazy"`), imágenes responsivas (`srcset`) y dimensiones explícitas para evitar saltos de layout.

## Notas relacionadas

- [[01 Imágenes/index]] — el medio más común.
- [[10 Accesibilidad (A11Y)/index]] — las alternativas textuales en profundidad.
