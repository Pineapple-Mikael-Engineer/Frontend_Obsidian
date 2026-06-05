---
title: Schema LocalBusiness — Negocios locales
aliases:
  - localbusiness schema
tags:
  - html
  - api/concepto
  - metadatos
draft: false
---

# LocalBusiness

> [!definicion]
> El tipo `LocalBusiness` (un subtipo de [[03 Person y Organization | `Organization`]]) describe un **negocio físico**: una tienda, un restaurante, una clínica. Aporta a Google la dirección, el horario, el teléfono y la geolocalización, claves para el **SEO local** y para aparecer en Google Maps y el "pack local".

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Restaurant",
  "name": "La Tasca de Ana",
  "image": "https://sitio.com/local.jpg",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Calle Mayor 1",
    "addressLocality": "Madrid",
    "postalCode": "28013",
    "addressCountry": "ES"
  },
  "telephone": "+34911234567",
  "openingHours": "Mo-Sa 13:00-23:00",
  "geo": { "@type": "GeoCoordinates", "latitude": 40.415, "longitude": -3.707 },
  "priceRange": "€€"
}
</script>
```

## Subtipos específicos

`LocalBusiness` tiene subtipos más concretos que conviene usar cuando aplican:

| `@type` | Para |
|---------|------|
| `Restaurant` | Restaurantes, cafeterías |
| `Store` | Tiendas |
| `MedicalBusiness` | Clínicas, consultas |
| `LodgingBusiness` | Hoteles |

## Propiedades clave

| Propiedad | Para qué |
|-----------|----------|
| `name`, `image` | Identidad del negocio |
| `address` (`PostalAddress`) | Dirección estructurada |
| `telephone` | Teléfono (formato internacional) |
| `openingHours` | Horario (`Mo-Fr 09:00-18:00`) |
| `geo` (`GeoCoordinates`) | Latitud y longitud |
| `priceRange` | Rango de precios (`€`, `€€`, `€€€`) |

## La coherencia NAP

> [!info] Nombre, dirección, teléfono consistentes
> En SEO local es crítica la coherencia **NAP** (Name, Address, Phone): el nombre, la dirección y el teléfono deben ser **idénticos** en el JSON-LD, en la página visible, en Google Business Profile y en los directorios. Discrepancias (una abreviatura distinta, otro teléfono) confunden a Google sobre si es el mismo negocio y debilitan el posicionamiento local.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa el subtipo más específico (`Restaurant`, `Store`…).
> - Estructura la dirección con `PostalAddress` y añade `geo`.
> - Mantén el NAP idéntico en web, JSON-LD y Google Business Profile.
> - Declara `openingHours` con el formato de Schema (`Mo-Sa 13:00-23:00`).

## Errores comunes

> [!warning] Trampas
> - **NAP inconsistente** entre la web, el dato estructurado y los directorios.
> - **`address` como texto plano** en vez de `PostalAddress` estructurada.
> - **Horario desactualizado** respecto a la realidad.

## Notas relacionadas

- [[03 Person y Organization]] — el tipo padre (`Organization`).
- [[10 Dirección (address)]] — el marcado HTML de los datos de contacto.
- [[01 Schema.org Básico]] — la sintaxis JSON-LD.
