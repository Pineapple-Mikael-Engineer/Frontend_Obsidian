---
title: Schema BreadcrumbList — Migas de pan
aliases:
  - breadcrumblist schema
  - breadcrumbs
tags:
  - html
  - api/concepto
  - metadatos
draft: false
---

# BreadcrumbList

> [!definicion]
> El tipo `BreadcrumbList` describe las **migas de pan** (breadcrumbs): la ruta jerárquica que sitúa la página dentro del sitio (Inicio › Categoría › Producto). Permite que Google muestre esa ruta **en lugar de la URL** en los resultados, más legible y orientadora.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    { "@type": "ListItem", "position": 1, "name": "Inicio", "item": "https://sitio.com" },
    { "@type": "ListItem", "position": 2, "name": "Zapatillas", "item": "https://sitio.com/zapatillas" },
    { "@type": "ListItem", "position": 3, "name": "Running X1" }
  ]
}
</script>
```

## Estructura

| Pieza | Función |
|-------|---------|
| `itemListElement` | Array ordenado de niveles |
| `ListItem` → `position` | Posición en la ruta (1, 2, 3…) |
| `ListItem` → `name` | Texto del nivel |
| `ListItem` → `item` | URL del nivel (se omite en el último, la página actual) |

## El último nivel no lleva URL

> [!info] La página actual no se enlaza a sí misma
> El **último** `ListItem` (la página en la que estás) suele llevar solo `name`, **sin** `item`/URL, porque es la página actual y no tiene sentido enlazarla a sí misma. Los niveles intermedios sí llevan su URL para que Google los muestre como enlaces en la ruta.

## Coherencia con las breadcrumbs visibles

> [!info] Acompaña a la navegación visible
> El `BreadcrumbList` describe la misma ruta que las migas de pan **visibles** en la página, marcadas con un [[03 Navegación (nav) | `<nav aria-label="Migas de pan">`]]. El JSON-LD las hace explícitas para Google; deben coincidir con lo que ve el usuario. No se trata de inventar una jerarquía solo para el dato estructurado.

## Buenas prácticas

> [!tip] Recomendaciones
> - Numera `position` desde 1, en orden de la raíz a la página actual.
> - Da `item` (URL) a todos los niveles salvo, opcionalmente, el último.
> - Haz coincidir la ruta con las breadcrumbs visibles del `<nav>`.
> - Refleja la jerarquía real del sitio, no una inventada.

## Errores comunes

> [!warning] Trampas
> - **`position` desordenado** o empezando en 0.
> - **Ruta que no coincide** con las breadcrumbs visibles.
> - **URL en el último nivel** apuntando a sí mismo (innecesario).

## Notas relacionadas

- [[03 Navegación (nav)]] — el marcado HTML de las migas de pan visibles.
- [[01 Schema.org Básico]] — la sintaxis y la coherencia con lo visible.
