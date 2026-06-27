---
title: Schema.org básico — Vocabulario y sintaxis JSON-LD
aliases:
  - schema.org
  - json-ld basics
tags:
  - html
  - api/concepto
  - metadatos
draft: false
order: 1
---

# Schema.org Básico

> [!definicion]
> **Schema.org** es el vocabulario compartido (creado por Google, Bing, Yahoo y Yandex) con el que se describen entidades del mundo real: artículos, productos, personas, eventos. En JSON-LD, cada bloque declara un tipo (`@type`) y sus propiedades, dentro del contexto de Schema.org.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Cómo regar un cactus",
  "author": { "@type": "Person", "name": "Ana López" },
  "datePublished": "2026-06-04"
}
</script>
```

## La estructura de un bloque JSON-LD

| Clave | Función |
|-------|---------|
| `@context` | Siempre `"https://schema.org"`: define el vocabulario |
| `@type` | El tipo de entidad (`Article`, `Product`, `Recipe`…) |
| Propiedades | Los datos del tipo (`name`, `author`, `price`…) |

## Tipos anidados

Las propiedades pueden contener **otros tipos**. Un `Article` tiene un `author` que es una `Person`, que a su vez puede tener una `Organization`:

```json
{
  "@type": "Article",
  "author": {
    "@type": "Person",
    "name": "Ana López",
    "worksFor": { "@type": "Organization", "name": "Diario X" }
  }
}
```

Esta composición permite describir relaciones ricas entre entidades.

## Dónde colocarlo

El bloque `<script type="application/ld+json">` va en el `<head>` o en el `<body>` (Google lee ambos). Es invisible para el usuario: solo lo leen los buscadores. Puede haber **varios** bloques en una página (un `Article` y un `BreadcrumbList`, por ejemplo).

## Propiedades obligatorias vs. recomendadas

> [!warning] Faltar una obligatoria invalida el rich result
> Cada tipo tiene propiedades **obligatorias** (sin ellas, Google no muestra el resultado enriquecido) y **recomendadas** (mejoran la presentación). Por ejemplo, un `Product` necesita al menos `name`; para las estrellas, además `review` o `aggregateRating`. La documentación de Google para cada tipo lista cuáles son críticas.

## Coherencia con el contenido visible

> [!warning] Los datos deben coincidir con la página
> Google penaliza los datos estructurados que **no reflejan** el contenido visible: poner un precio o una valoración en el JSON-LD que no aparecen en la página es contra sus directrices y puede acarrear una acción manual. El JSON-LD describe lo que el usuario ve, no datos inventados para ganar estrellas.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa JSON-LD (recomendado por Google) sobre microdata/RDFa.
> - Incluye las propiedades **obligatorias** del tipo; añade las recomendadas que apliquen.
> - Asegura que los datos **coinciden** con el contenido visible.
> - Valida con la Prueba de resultados enriquecidos antes de publicar.

## Errores comunes

> [!warning] Trampas
> - **Olvidar `@context` o `@type`**: el bloque no se interpreta.
> - **JSON mal formado** (una coma de más): Google ignora todo el bloque.
> - **Datos que no están en la página**: contra las directrices, riesgo de penalización.

## Notas relacionadas

- [[index]] — el panorama de datos estructurados.
- [[02 Article]] · [[04 Product]] · [[06 Recipe]] — los tipos concretos más usados.
