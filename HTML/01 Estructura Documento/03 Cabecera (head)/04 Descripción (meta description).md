---
title: <meta description> — Resumen de la página para buscadores
aliases:
  - meta description
tags:
  - html
  - api/elemento
  - metadatos
elemento: meta
categoria: metadatos
rol_implicito: ninguno
vacio: true
draft: false
order: 4
---

# Descripción (meta description)

> [!definicion]
> `<meta name="description">` aporta un **resumen** de la página. Google suele mostrarlo como el texto
> gris (snippet) bajo el titular en los resultados de búsqueda. No influye directamente en el ranking,
> pero **sí en el porcentaje de clics** (CTR): es, en la práctica, el texto publicitario de la página
> en el buscador.

```html
<meta name="description"
      content="Aprende a hacer tarta de manzana casera en 45 minutos con 6 ingredientes. Receta paso a paso con fotos." />
```

## Cómo lo trata Google

Es importante entender que la `description` es una **sugerencia, no una garantía**:

- Si Google considera que tu descripción resume bien la página **para la consulta concreta**, la usa.
- Si cree que otro fragmento del contenido responde mejor a la búsqueda, **genera su propio snippet**
  e ignora la etiqueta.

Por eso una misma página puede mostrar descripciones distintas según lo que el usuario buscó. Aun
así, conviene escribirla: es la mejor apuesta por defecto y la única sobre la que tienes control.

## Cómo redactarla

| Pauta | Razón |
|-------|-------|
| 150–160 caracteres | Google trunca el snippet alrededor de ese límite |
| Una por página, única | Descripciones duplicadas diluyen la relevancia entre páginas |
| Incluye la propuesta de valor | Es un anuncio: debe invitar al clic |
| Refleja el contenido real | Una descripción engañosa sube el CTR pero hunde el tiempo en página |
| Sin comillas dobles sin escapar | Google corta el texto en la primera `"` |

### Ejemplo bueno vs. flojo

```html
<!-- Flojo: genérico, no invita -->
<meta name="description" content="Página de recetas." />

<!-- Bueno: específico, con beneficio y llamada implícita -->
<meta name="description"
      content="Receta de tarta de manzana casera en 45 min con 6 ingredientes. Paso a paso, con fotos y trucos para que quede jugosa." />
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Escribe una descripción **distinta y específica** para cada página importante.
> - Trátala como copy de marketing: incluye el beneficio principal en los primeros 120 caracteres por
>   si se trunca.
> - En sitios grandes, genera descripciones a partir de campos reales (extracto del artículo), nunca
>   la misma plantilla repetida.

## Errores comunes

> [!warning] Trampas
> - **No confundir con `keywords`**: [[05 Palabras Clave (meta keywords) | `meta keywords`]] está
>   muerta para SEO; la `description` sigue viva como herramienta de CTR.
> - **Descripción duplicada** en todo el sitio: Google Search Console la marca como problema.
> - **Description que no coincide con el contenido**: aumenta el rebote y, a la larga, la reputación
>   del dominio.

## Notas relacionadas

- [[03 Título del Documento (title)]] — el titular que acompaña a esta descripción en el resultado.
- [[05 Palabras Clave (meta keywords)]] — el `meta` hermano, ya en desuso.
- [[09 Metadatos y SEO/index]] — el panorama completo de SEO técnico.
