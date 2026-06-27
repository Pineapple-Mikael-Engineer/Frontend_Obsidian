---
title: Schema Product — Productos con precio y valoración
aliases:
  - product schema
tags:
  - html
  - api/concepto
  - metadatos
draft: false
order: 4
---

# Product

> [!definicion]
> El tipo `Product` describe un **producto** a la venta. Combinado con `Offer` (precio y disponibilidad) y `AggregateRating`/`Review` (valoraciones), habilita uno de los rich results más valiosos: el resultado con **precio, disponibilidad y estrellas** bajo el enlace.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Zapatillas de running X1",
  "image": "https://sitio.com/x1.jpg",
  "description": "Zapatillas ligeras para asfalto.",
  "brand": { "@type": "Brand", "name": "RunFast" },
  "offers": {
    "@type": "Offer",
    "price": "89.99",
    "priceCurrency": "EUR",
    "availability": "https://schema.org/InStock"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.6",
    "reviewCount": "127"
  }
}
</script>
```

## Las tres piezas

| Pieza | Aporta al resultado |
|-------|---------------------|
| `Product` | Nombre, imagen, marca, descripción |
| `offers` (`Offer`) | Precio, moneda, disponibilidad |
| `aggregateRating` / `review` | Las estrellas y el número de reseñas |

## Offer: precio y disponibilidad

| Propiedad de `Offer` | Valor |
|----------------------|-------|
| `price` | El precio (número, sin símbolo de moneda) |
| `priceCurrency` | Código ISO (`EUR`, `USD`) |
| `availability` | URL de Schema: `InStock`, `OutOfStock`, `PreOrder` |
| `priceValidUntil` | Fecha hasta la que el precio es válido |

## Las estrellas exigen reseñas reales

> [!warning] No inventes valoraciones
> Las estrellas (`aggregateRating`) son muy llamativas y tentadoras, pero Google exige que correspondan a **reseñas reales y visibles** en la página. Marcar `aggregateRating` sin reseñas visibles, o autovalorarse, viola las directrices y puede acarrear una **acción manual** que retire todos los rich results del sitio. Solo se marcan valoraciones genuinas de usuarios mostradas en la página.

## Buenas prácticas

> [!tip] Recomendaciones
> - Incluye `name`, `image`, `offers` con `price`/`priceCurrency`/`availability`.
> - Añade `brand` y `description`.
> - Marca `aggregateRating` solo si hay reseñas reales visibles.
> - Mantén el precio del JSON-LD sincronizado con el de la página.

## Errores comunes

> [!warning] Trampas
> - **Valoraciones inventadas**: riesgo de penalización.
> - **`price` con símbolo de moneda** (`89,99 €`): debe ser solo el número, con `priceCurrency` aparte.
> - **Precio desactualizado** respecto a la página.

## Notas relacionadas

- [[01 Schema.org Básico]] — la sintaxis y la coherencia con el contenido.
- [[09 LocalBusiness]] — para negocios, no productos.
