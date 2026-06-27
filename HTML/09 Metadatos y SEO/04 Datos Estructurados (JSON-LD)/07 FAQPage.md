---
title: Schema FAQPage — Preguntas frecuentes
aliases:
  - faqpage schema
  - faq
tags:
  - html
  - api/concepto
  - metadatos
draft: false
order: 7
---

# FAQPage

> [!definicion]
> El tipo `FAQPage` marca una página de **preguntas frecuentes**: una lista de preguntas con sus respuestas oficiales. Permitía que Google mostrara las preguntas **desplegables directamente en el resultado de búsqueda**, ocupando mucho más espacio en la página de resultados.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "¿Cuánto tarda el envío?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Entre 24 y 48 horas laborables en península."
      }
    },
    {
      "@type": "Question",
      "name": "¿Puedo devolver un producto?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí, dispones de 30 días para devoluciones."
      }
    }
  ]
}
</script>
```

## Estructura

| Pieza | Función |
|-------|---------|
| `mainEntity` | Array de preguntas |
| `Question` → `name` | El texto de la pregunta |
| `acceptedAnswer` → `Answer` → `text` | La respuesta (admite HTML básico) |

## Requisito: solo FAQ oficiales

> [!warning] No es para Q&A de usuarios ni para acumular keywords
> `FAQPage` es para preguntas frecuentes **escritas por el sitio** con respuestas oficiales. **No** vale para:
> - Foros donde los usuarios responden (eso es `QAPage`).
> - Páginas que meten "FAQ" artificiales solo para ganar espacio en los resultados.
>
> Además, las preguntas y respuestas del JSON-LD deben **estar visibles** en la página, no ocultas solo para el dato estructurado.

## Cambios en su visibilidad

> [!info] Google redujo su aparición
> Google ha **restringido** la aparición de los rich results de FAQ (en su momento muy abundantes, ahora limitados a sitios oficiales/gubernamentales y de salud en muchos casos). Aun así, marcar correctamente la FAQ sigue siendo buena práctica: documenta el contenido y puede mostrarse según la evolución del algoritmo. No te bases solo en ganar el rich result.

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsalo solo para FAQ **oficiales** del sitio, visibles en la página.
> - Cada `Question` con una sola `acceptedAnswer` clara.
> - No infles la página con preguntas artificiales.
> - Para preguntas y respuestas de comunidad, usa `QAPage`, no `FAQPage`.

## Errores comunes

> [!warning] Trampas
> - **FAQ inventadas** para ganar espacio: contra las directrices.
> - **Contenido del JSON-LD no visible** en la página.
> - **Confundir `FAQPage` con `QAPage`** (este último es para foros/comunidad).

## Notas relacionadas

- [[04 Listas de Definición (dl, dt, dd)]] — el marcado HTML idiomático de pares pregunta–respuesta.
- [[01 Schema.org Básico]] — la sintaxis y la coherencia con lo visible.
