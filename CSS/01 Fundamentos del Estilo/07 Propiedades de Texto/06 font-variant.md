---
title: font-variant — Versalitas y variantes tipográficas
aliases:
  - font-variant
  - small-caps
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: font-variant
grupo: tipografia
valor_inicial: normal
hereda: true
animable: false
draft: false
order: 6
---

# font-variant

> [!definicion]
> `font-variant` controla **variantes tipográficas** que la fuente puede ofrecer: versalitas (small-caps), cifras especiales, ligaduras, fracciones. Originalmente solo activaba versalitas; hoy es un shorthand de varias propiedades `font-variant-*` más específicas.

```css
.titulo { font-variant: small-caps; }   /* VERSALITAS */
```

## Las versalitas (small-caps)

El uso clásico: `small-caps` convierte las minúsculas en **mayúsculas de menor tamaño**, conservando las mayúsculas reales a tamaño normal. Da un aspecto elegante a títulos, siglas y encabezados:

```css
.sigla { font-variant: small-caps; }
/* "OMS lanza..." → versalitas finas */
```

A diferencia de [[10 text-transform | `text-transform: uppercase`]] (que convierte **todo** a mayúsculas grandes), las versalitas mantienen la distinción visual entre lo que era mayúscula y lo que era minúscula.

## Las sub-propiedades modernas

`font-variant` agrupa hoy varias propiedades específicas, más potentes si la fuente las soporta (OpenType features):

| Propiedad | Controla |
|-----------|----------|
| `font-variant-caps` | Versalitas (`small-caps`, `all-small-caps`…) |
| `font-variant-numeric` | Cifras: `tabular-nums`, `oldstyle-nums`, `fraction` |
| `font-variant-ligatures` | Ligaduras (`fi`, `fl`…) |
| `font-variant-position` | Super/subíndices tipográficos |

### tabular-nums: cifras alineadas

> [!tip] Números que se alinean en columnas
> `font-variant-numeric: tabular-nums` hace que todos los dígitos ocupen el **mismo ancho**, lo que alinea perfectamente los números en tablas y precios (las unidades, decenas y centenas quedan en columna):
> ```css
> td.precio { font-variant-numeric: tabular-nums; text-align: right; }
> ```
> Imprescindible en tablas de datos numéricos.

## Depende de la fuente

> [!warning] Las variantes requieren soporte OpenType de la fuente
> Las variantes avanzadas (versalitas reales, ligaduras, cifras de estilo antiguo) solo funcionan si la **fuente las incluye** como *features* OpenType. Si no, el navegador puede sintetizar versalitas (encogiendo mayúsculas, peor resultado) o ignorar la variante. Las fuentes de calidad las traen; las de sistema, no siempre.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `small-caps` para siglas y encabezados elegantes (mejor que `uppercase` cuando quieras versalitas).
> - `tabular-nums` en columnas de números (tablas, precios).
> - Las sub-propiedades `font-variant-*` dan más control que el shorthand.
> - Verifica que la fuente soporte la variante antes de depender de ella.

## Errores comunes

> [!warning] Trampas
> - **Esperar versalitas reales** de una fuente que no las tiene: salen sintéticas.
> - **Confundir `small-caps` con `uppercase`**: el segundo agranda todo.
> - **Números desalineados** en tablas por no usar `tabular-nums`.

## Notas relacionadas

- [[10 text-transform]] — convertir a mayúsculas (distinto de versalitas).
- [[02 font-feature-settings]] — control de bajo nivel de features OpenType.
- [[04 Celda de Datos (td)]] — donde `tabular-nums` brilla.
