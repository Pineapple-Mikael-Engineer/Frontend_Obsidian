---
title: Schema Recipe — Recetas de cocina
aliases:
  - recipe schema
tags:
  - html
  - api/concepto
  - metadatos
draft: false
order: 6
---

# Recipe

> [!definicion]
> El tipo `Recipe` describe una **receta de cocina**. Es uno de los rich results más completos: Google muestra la foto, el tiempo de preparación, las calorías, la valoración y, en búsquedas por voz, puede leer los pasos. Es casi imprescindible para cualquier sitio de cocina.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Recipe",
  "name": "Tarta de manzana",
  "image": "https://sitio.com/tarta.jpg",
  "author": { "@type": "Person", "name": "Ana López" },
  "datePublished": "2026-06-04",
  "prepTime": "PT20M",
  "cookTime": "PT25M",
  "totalTime": "PT45M",
  "recipeYield": "8 porciones",
  "recipeIngredient": [
    "3 manzanas", "200 g de harina", "2 huevos"
  ],
  "recipeInstructions": [
    { "@type": "HowToStep", "text": "Precalentar el horno a 180 °C." },
    { "@type": "HowToStep", "text": "Mezclar los ingredientes secos." }
  ],
  "nutrition": { "@type": "NutritionInformation", "calories": "320 kcal" }
}
</script>
```

## Propiedades clave

| Propiedad | Para qué |
|-----------|----------|
| `name`, `image` | Nombre y foto (la foto es muy destacada) |
| `prepTime` / `cookTime` / `totalTime` | Tiempos en formato de duración ISO (`PT45M`) |
| `recipeYield` | Cuántas porciones |
| `recipeIngredient` | Lista de ingredientes (texto) |
| `recipeInstructions` | Pasos, idealmente como `HowToStep` |
| `nutrition` | Información nutricional (calorías…) |
| `aggregateRating` | Valoración (si hay reseñas reales) |

## Los tiempos en formato ISO 8601

> [!info] PT45M, no "45 minutos"
> Los tiempos van en **formato de duración ISO 8601**, no en texto libre: `PT` (period of time) seguido de la duración. `PT45M` = 45 minutos; `PT1H30M` = 1 h 30 min. El texto "45 minutos" no es válido para el dato estructurado (aunque sí se muestre así al usuario en la página).

## Pasos como HowToStep

Marcar cada paso como `HowToStep` (en vez de un bloque de texto) permite a Google y a los asistentes de voz **leer los pasos uno a uno** ("siguiente paso"), una gran ventaja para recetas en la cocina.

## Buenas prácticas

> [!tip] Recomendaciones
> - Incluye `image` en alta calidad (es lo más visible del rich result).
> - Tiempos en formato ISO (`PT...`).
> - Marca los pasos como `HowToStep` para la lectura por voz.
> - Añade `nutrition` y `aggregateRating` (con reseñas reales) para enriquecer.

## Errores comunes

> [!warning] Trampas
> - **Tiempos en texto** ("45 min") en vez de ISO (`PT45M`).
> - **Instrucciones en un solo bloque** sin `HowToStep`: se pierde la lectura por pasos.
> - **Valoraciones inventadas**: contra las directrices.

## Notas relacionadas

- [[23 Tiempo (time)]] — duraciones ISO también en HTML.
- [[01 Listas Ordenadas (ol)]] — los pasos visibles van en una lista ordenada.
- [[01 Schema.org Básico]] — la sintaxis JSON-LD.
