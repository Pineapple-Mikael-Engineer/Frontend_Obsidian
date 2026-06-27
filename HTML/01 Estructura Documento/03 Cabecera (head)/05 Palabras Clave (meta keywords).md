---
title: <meta keywords> — Palabras clave (obsoleta para SEO)
aliases:
  - meta keywords
tags:
  - html
  - api/elemento
  - metadatos
elemento: meta
categoria: metadatos
rol_implicito: ninguno
vacio: true
draft: false
order: 5
---

# Palabras Clave (meta keywords)

> [!definicion]
> `<meta name="keywords">` enumeraba las palabras clave de la página separadas por comas. **Google la
> ignora desde 2009** y Bing la trata como posible señal de spam. Hoy es marcado muerto para
> posicionamiento: se documenta para **reconocerla y descartarla**, no para usarla.

```html
<meta name="keywords" content="recetas, tarta, manzana, postre, casero" />
```

## Por qué murió

En los primeros buscadores, `keywords` influía en el ranking, así que se llenó de abuso: webmasters
metían cientos de términos irrelevantes (*keyword stuffing*) para colarse en cualquier búsqueda.
Como era trivial de manipular y no reflejaba la calidad real del contenido, los motores la
abandonaron como factor. Es el ejemplo canónico de un metadato deprecado **por abuso**, no por fallo
técnico.

## Qué hacer en su lugar

El esfuerzo de SEO va a señales que sí importan y que son difíciles de falsear:

| En vez de keywords | Usar |
|--------------------|------|
| Listar términos | Un `<title>` bien redactado |
| Repetir palabras clave | Una `description` atractiva |
| Meta tags | Contenido real y encabezados `<h1>`–`<h6>` |
| — | Datos estructurados (JSON-LD) |

El detalle de cada alternativa está en [[03 Título del Documento (title)]],
[[04 Descripción (meta description)]] y [[04 Datos Estructurados (JSON-LD)/index | los datos estructurados]].

> [!warning] Inofensiva pero inútil
> Rellenarla no penaliza en Google (simplemente la ignora), pero tampoco aporta nada. El tiempo
> invertido en ella es tiempo perdido. Si aparece en una plantilla heredada, puede eliminarse sin
> consecuencias.

> [!info] Excepción: buscadores internos
> Algún motor de búsqueda **interno** de un CMS concreto puede leer este campo para indexar su propio
> contenido. Es el único contexto donde sigue teniendo algún uso, y siempre específico de esa
> herramienta.

## Notas relacionadas

- [[04 Descripción (meta description)]] — el `meta` que **sí** sigue vigente.
- [[09 Metadatos y SEO/index]] — qué metadatos importan hoy.
