---
title: <time> — Fecha u hora legible por máquina
aliases:
  - time
  - datetime
tags:
  - html
  - api/elemento
  - semantica
elemento: time
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
---

# Tiempo (time)

> [!definicion]
> `<time>` marca una **fecha, hora o duración** en un formato que las máquinas pueden leer, mientras muestra al usuario un texto natural. El atributo `datetime` contiene la versión estandarizada (ISO 8601); el contenido del elemento, la versión humana.

```html
<p>Publicado el <time datetime="2026-06-04">4 de junio de 2026</time>.</p>
```

El usuario lee "4 de junio de 2026"; el navegador, los buscadores y los calendarios leen `2026-06-04`.

## El atributo datetime

`datetime` acepta varios formatos ISO 8601 según lo que se exprese:

| Qué se expresa | Formato `datetime` | Ejemplo |
|----------------|--------------------|---------|
| Fecha | `AAAA-MM-DD` | `2026-06-04` |
| Hora | `hh:mm` | `14:30` |
| Fecha y hora | `AAAA-MM-DDThh:mm` | `2026-06-04T14:30` |
| Fecha y hora con zona | `…±hh:mm` | `2026-06-04T14:30+02:00` |
| Año-mes | `AAAA-MM` | `2026-06` |
| Duración | `PnYnMnDTnHnMnS` | `PT2H30M` (2 h 30 min) |

```html
<p>El evento dura <time datetime="PT90M">hora y media</time>.</p>
<p>Nos vemos a las <time datetime="20:00">8 de la tarde</time>.</p>
```

## Por qué importa: datos legibles por máquina

> [!info] Para qué sirve realmente
> El valor de `<time>` está en que un texto como "ayer", "el próximo martes" o "4 jun" es ambiguo para una máquina. El `datetime` da la fecha exacta y sin ambigüedad, lo que permite:
> - A los **buscadores**, mostrar la fecha de publicación en los resultados (y ordenar por recencia).
> - A los **lectores de feeds** y agregadores, fechar el contenido correctamente.
> - A scripts y extensiones, ofrecer "añadir al calendario" o convertir zonas horarias.

## Uso en artículos

`<time>` es habitual en la cabecera de un [[06 Artículos (article) | `<article>`]] para marcar la publicación, a menudo combinado con datos estructurados:

```html
<article>
  <header>
    <h2>Cómo regar un cactus</h2>
    <p>Por Ana · <time datetime="2026-06-04">hace 2 días</time></p>
  </header>
</article>
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon siempre el `datetime` en ISO 8601 válido, aunque el texto visible sea informal ("ayer").
> - Inclúyelo en la fecha de publicación de artículos y eventos.
> - Para eventos, considera complementarlo con JSON-LD `Event` para resultados enriquecidos.

## Errores comunes

> [!warning] Trampas
> - **`datetime` con formato libre** (`4/6/26`): debe ser ISO 8601 o se ignora.
> - **Texto ambiguo sin `datetime`**: "ayer" sin el atributo no aporta dato máquina-legible.
> - **Usar `<time>` para texto que no es una fecha/hora/duración**: solo aplica a instantes y periodos.

## Notas relacionadas

- [[06 Artículos (article)]] — donde `<time>` marca la fecha de publicación.
- [[09 Metadatos y SEO/index]] — los datos estructurados que complementan a `<time>`.
