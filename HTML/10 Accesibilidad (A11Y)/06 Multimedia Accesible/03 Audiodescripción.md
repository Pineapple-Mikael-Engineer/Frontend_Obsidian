---
title: Audiodescripción — Narrar lo visual para personas ciegas
aliases:
  - audio description
  - audiodescripción
tags:
  - html
  - api/concepto
  - a11y
draft: false
---

# Audiodescripción

> [!definicion]
> La **audiodescripción** narra la información **visual** de un vídeo —acciones, escenarios, gestos, texto en pantalla— para personas ciegas o con baja visión. Se intercala en las **pausas naturales** del diálogo, contando lo que ocurre cuando nadie habla. Es la contraparte de los [[01 Subtítulos (captions) | subtítulos]]: estos dan el audio a quien no oye; la audiodescripción da lo visual a quien no ve.

## El problema que resuelve

> [!info] El diálogo no cuenta toda la historia
> Imagina una escena: dos personajes se miran en silencio, uno saca una carta y la rompe. Sin diálogo, una persona ciega no se entera de **nada** de lo que pasa. La audiodescripción rellena esos huecos: *"Marta mira fijamente a Luis. Saca una carta del bolsillo y la rompe en dos"*. Sin ella, gran parte de la narrativa visual se pierde.

## Cuándo es imprescindible

| Contenido | ¿Necesita audiodescripción? |
|-----------|------------------------------|
| Vídeo con acción visual no narrada | Sí |
| Vídeo donde el diálogo describe todo | Menos crítica |
| "Cabeza parlante" (alguien hablando a cámara) | Poco necesaria |
| Texto importante solo en pantalla | Sí (o léelo en el audio) |

## Tipos

| Tipo | Cómo |
|------|------|
| **Estándar** | Se inserta en las pausas del diálogo existentes |
| **Extendida** | El vídeo se **pausa** para describir cuando no hay pausas suficientes |

La extendida es más completa pero interrumpe el ritmo; se usa cuando la acción es tan densa que no caben descripciones en las pausas naturales.

## Implementación en HTML

La audiodescripción puede ofrecerse como:

- Una **pista de audio alternativa** del vídeo (versión con descripción).
- Una pista [[04 Pistas de Texto (track) | `<track kind="descriptions">`]], que el lector de pantalla lee en voz alta en los momentos indicados.
- Una **versión separada** del vídeo con la descripción ya mezclada.
- Cubierta por una [[02 Transcripciones | transcripción descriptiva]] como alternativa.

## La alternativa práctica: transcripción descriptiva

> [!tip] Cuando producir audiodescripción es inviable
> Producir audiodescripción profesional es costoso. Una alternativa razonable, sobre todo para contenido sencillo, es una **transcripción descriptiva** completa (diálogo + descripción de lo visual) junto al vídeo. No sustituye del todo a la audiodescripción sincronizada para contenido complejo, pero hace accesible la información visual a un coste mucho menor.

## Buenas prácticas

> [!tip] Recomendaciones
> - Prioriza la audiodescripción en vídeos con información visual no narrada.
> - Describe lo **relevante** para la comprensión, no cada detalle.
> - Para texto en pantalla importante, asegúrate de que se lea o describa.
> - Si la producción es inviable, ofrece al menos una transcripción descriptiva.

## Errores comunes

> [!warning] Trampas
> - **Ignorar lo visual** asumiendo que el diálogo basta.
> - **No describir el texto en pantalla** (rótulos, datos que solo aparecen visualmente).
> - **Sobrecargar** la descripción hasta pisar el diálogo.

## Notas relacionadas

- [[01 Subtítulos (captions)]] — la contraparte: el audio para quien no oye.
- [[02 Transcripciones]] — la transcripción descriptiva como alternativa.
- [[04 Pistas de Texto (track)]] — `kind="descriptions"`.
