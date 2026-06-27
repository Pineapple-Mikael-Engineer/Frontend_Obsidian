---
title: Datos Estructurados (JSON-LD) — Describir el contenido para buscadores
aliases:
  - structured data
  - json-ld
  - schema.org
tags:
  - html
  - api/concepto
  - metadatos
draft: false
order: 4
---

# Datos Estructurados (JSON-LD)

> [!definicion]
> Los **datos estructurados** describen el contenido de la página en un vocabulario que los buscadores entienden ([[01 Schema.org Básico | Schema.org]]), habilitando **resultados enriquecidos**: estrellas de reseña, precios, FAQ desplegables, recetas con foto y tiempo. El formato recomendado por Google es **JSON-LD**: un bloque `<script>` con un objeto JSON.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Recipe",
  "name": "Tarta de manzana",
  "totalTime": "PT45M"
}
</script>
```

## Por qué JSON-LD y no microdata

> [!info] JSON-LD: separado del HTML
> Hay tres formas de añadir datos estructurados (microdata, RDFa, JSON-LD). Google **recomienda JSON-LD** porque va en un bloque `<script>` **aparte**, sin mezclarse con el marcado del contenido. Esto lo hace fácil de generar, mantener y validar, sin ensuciar el HTML con atributos `itemprop` por todas partes. Es el estándar de facto.

## Mapa de la sección

- [[01 Schema.org Básico]] — el vocabulario, la sintaxis JSON-LD y cómo validar.
- [[02 Article]] — artículos y noticias.
- [[03 Person y Organization]] — autores, empresas, perfiles.
- [[04 Product]] — productos con precio y disponibilidad.
- [[05 Event]] — eventos con fecha y lugar.
- [[06 Recipe]] — recetas con ingredientes y tiempos.
- [[07 FAQPage]] — preguntas frecuentes desplegables.
- [[08 BreadcrumbList]] — migas de pan en los resultados.
- [[09 LocalBusiness]] — negocios locales con dirección y horario.

## Lo que se gana: rich results

| Tipo | Resultado enriquecido en Google |
|------|--------------------------------|
| `Product` + `Review` | Estrellas y precio bajo el resultado |
| `Recipe` | Foto, tiempo, calorías, valoración |
| `FAQPage` | Preguntas desplegables en el propio resultado |
| `BreadcrumbList` | Migas de pan en lugar de la URL |
| `Event` | Fecha y lugar destacados |

## Validar siempre

> [!tip] Usa el validador
> Antes de confiar en los datos estructurados, valídalos con la **Prueba de resultados enriquecidos** de Google y el validador de Schema.org. Un error de sintaxis o una propiedad obligatoria ausente hace que Google ignore todo el bloque, sin avisar en la página.

## Notas relacionadas

- [[01 Schema.org Básico]] — el punto de partida.
- [[index]] — el panorama de metadatos y SEO.
