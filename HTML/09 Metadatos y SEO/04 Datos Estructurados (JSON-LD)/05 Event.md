---
title: Schema Event — Eventos con fecha y lugar
aliases:
  - event schema
tags:
  - html
  - api/concepto
  - metadatos
draft: false
---

# Event

> [!definicion]
> El tipo `Event` describe un **evento**: un concierto, una conferencia, un webinar, una función. Habilita que Google muestre la fecha, el lugar y el enlace de entradas de forma destacada, e incluso lo incluya en sus paneles de eventos.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Event",
  "name": "Concierto de jazz en directo",
  "startDate": "2026-07-15T21:00:00+02:00",
  "endDate": "2026-07-15T23:30:00+02:00",
  "eventAttendanceMode": "https://schema.org/OfflineEventAttendanceMode",
  "location": {
    "@type": "Place",
    "name": "Teatro Central",
    "address": "Calle Mayor 1, Madrid"
  },
  "offers": {
    "@type": "Offer",
    "price": "25.00",
    "priceCurrency": "EUR",
    "url": "https://sitio.com/entradas"
  }
}
</script>
```

## Propiedades clave

| Propiedad | Para qué |
|-----------|----------|
| `name` | Nombre del evento |
| `startDate` / `endDate` | Fechas en ISO 8601, **con zona horaria** |
| `location` | Un `Place` (presencial) o `VirtualLocation` (online) |
| `eventAttendanceMode` | Presencial, online o híbrido |
| `offers` | Entradas: precio y URL de compra |
| `organizer` | Quién organiza (una `Organization`) |

## Presencial, online o híbrido

`eventAttendanceMode` distingue el formato, lo que cambia qué `location` corresponde:

| Modo | `location` |
|------|------------|
| `OfflineEventAttendanceMode` | Un `Place` con dirección física |
| `OnlineEventAttendanceMode` | Un `VirtualLocation` con `url` |
| `MixedEventAttendanceMode` | Ambos |

## La zona horaria es crítica

> [!warning] startDate sin zona es ambiguo
> Un evento es global: alguien lo ve desde otra zona horaria. Por eso `startDate`/`endDate` deben incluir el **offset de zona** (`2026-07-15T21:00:00+02:00`), no solo fecha y hora. Sin él, Google puede mostrar una hora equivocada a usuarios de otras zonas. Es el error más común con eventos.

## Buenas prácticas

> [!tip] Recomendaciones
> - Incluye `startDate` con zona horaria; añade `endDate`.
> - Especifica `eventAttendanceMode` y el `location` adecuado.
> - Añade `offers` con la URL de entradas y `organizer`.
> - Para eventos cancelados/aplazados, usa `eventStatus`.

## Errores comunes

> [!warning] Trampas
> - **`startDate` sin zona horaria**: hora ambigua entre usuarios.
> - **`location` físico en un evento online** (o al revés).
> - **Eventos pasados** sin actualizar: ensucian los resultados.

## Notas relacionadas

- [[23 Tiempo (time)]] — el marcado HTML de fechas (formato ISO compartido).
- [[03 Person y Organization]] — el `organizer`.
- [[01 Schema.org Básico]] — la sintaxis JSON-LD.
