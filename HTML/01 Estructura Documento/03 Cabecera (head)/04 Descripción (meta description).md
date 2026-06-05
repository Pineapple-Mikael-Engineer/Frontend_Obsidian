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
---

# Descripción (meta description)

> [!definicion]
> `<meta name="description">` aporta un **resumen** de la página. Google suele usarlo como el texto
> gris (snippet) bajo el titular en los resultados de búsqueda. No influye directamente en el
> ranking, pero **sí en el porcentaje de clics**.

```html
<meta name="description"
      content="Aprende a hacer tarta de manzana casera en 45 minutos con 6 ingredientes. Receta paso a paso con fotos." />
```

## Pautas

| Pauta | Razón |
|-------|-------|
| 150–160 caracteres | Google trunca el snippet alrededor de ese límite |
| Una por página, única | Descripciones duplicadas diluyen la relevancia |
| Incluir la propuesta de valor | Es un anuncio: invita al clic |
| Sin comillas dobles sin escapar | Google corta el texto en la primera comilla |

> [!info] Google puede ignorarla
> El buscador no está obligado a usar esta descripción: a menudo genera su propio snippet a partir
> del contenido si lo considera más relevante para la consulta. Aun así, conviene escribirla: es la
> mejor apuesta por defecto.

> [!tip] No la confundas con las palabras clave
> La [[05 Palabras Clave (meta keywords) | `meta keywords`]] está obsoleta para SEO; la
> `description` **sí** sigue vigente como herramienta de marketing en el resultado de búsqueda.

## Notas relacionadas

- [[03 Título del Documento (title)]] — el titular que acompaña a esta descripción.
- [[05 Palabras Clave (meta keywords)]] — el `meta` hermano, ya en desuso.
- [[09 Metadatos y SEO/index]] — el panorama completo de SEO.
