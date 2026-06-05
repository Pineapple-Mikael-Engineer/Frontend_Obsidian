---
title: Transcripciones — Todo el contenido en texto
aliases:
  - transcripts
  - transcripciones
tags:
  - html
  - api/concepto
  - a11y
draft: false
---

# Transcripciones

> [!definicion]
> Una **transcripción** es la versión en **texto completo** de un contenido de audio o vídeo: todo el diálogo y, en una transcripción descriptiva, también lo visual relevante. Es la alternativa accesible imprescindible para el [[02 Audio (audio) | audio]] (que no admite subtítulos) y un complemento muy valioso para el vídeo.

```html
<audio controls>
  <source src="podcast.mp3" type="audio/mpeg" />
</audio>
<details>
  <summary>Transcripción del episodio</summary>
  <p><strong>Ana:</strong> Bienvenidos al pódcast…</p>
  <p><strong>Luis:</strong> Gracias por invitarme…</p>
</details>
```

## A quién beneficia

> [!info] Útil para muchos más que las personas sordas
> La transcripción ayuda a:
> - **Personas sordas** o con pérdida auditiva (su alternativa principal en audio).
> - Quien **prefiere leer** o no puede poner sonido (en una oficina, en transporte).
> - **Buscadores**: indexan el texto, mejorando el SEO del contenido multimedia.
> - Quien quiere **buscar** una frase concreta o citar el contenido.
>
> Es de las alternativas con mayor retorno: una sola transcripción sirve a públicos muy distintos.

## Transcripción simple vs. descriptiva

| Tipo | Incluye |
|------|---------|
| **Simple** | El diálogo y los sonidos relevantes (suficiente para audio) |
| **Descriptiva** | Además, lo **visual** importante (para vídeo: "Ana señala el gráfico") |

Para vídeo, la transcripción descriptiva combina el contenido de los [[01 Subtítulos (captions) | subtítulos]] y la [[03 Audiodescripción | audiodescripción]] en un solo texto, lo que la hace muy completa.

## Dónde y cómo colocarla

- **Cerca del reproductor**, claramente vinculada a él.
- A menudo dentro de un [[14 Web Components (slot, part, exportparts) | `<details>`]]/`<summary>` para que no ocupe espacio plegada pero esté disponible.
- Con la **estructura** del contenido: identificar hablantes, párrafos, marcas de tiempo si ayuda.

## Buenas prácticas

> [!tip] Recomendaciones
> - Ofrece transcripción para **todo** audio (podcasts, entrevistas).
> - Para vídeo, una transcripción descriptiva complementa muy bien a los subtítulos.
> - Colócala cerca del medio, con estructura (hablantes, párrafos).
> - Aprovecha el beneficio SEO: el texto se indexa.

## Errores comunes

> [!warning] Trampas
> - **Audio sin transcripción**: inaccesible para personas sordas (no hay subtítulos en audio).
> - **Transcripción como un único muro de texto** sin hablantes ni estructura.
> - **Esconderla** lejos del reproductor o sin enlace claro.

## Notas relacionadas

- [[02 Audio (audio)]] — el medio que **depende** de la transcripción.
- [[01 Subtítulos (captions)]] — la alternativa sincronizada del vídeo.
- [[03 Audiodescripción]] — la alternativa para lo visual.
