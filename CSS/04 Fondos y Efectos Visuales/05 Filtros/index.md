---
title: Filtros — Efectos gráficos sobre elementos
aliases:
  - css filters
tags:
  - css
  - api/concepto
  - fondos
draft: false
order: 4
---

# Filtros

> [!definicion]
> Los **filtros** aplican efectos gráficos a un elemento, como los de un editor de imágenes: desenfoque, brillo, contraste, escala de grises, sombras. [[01 filter | `filter`]] afecta al **propio elemento**; [[02 backdrop-filter | `backdrop-filter`]] afecta a lo que hay **detrás** (el clásico "cristal esmerilado").

```css
.borrosa { filter: blur(4px); }
.cristal { backdrop-filter: blur(10px); }
```

## Las dos propiedades

| Propiedad | Aplica el efecto a… | Uso típico |
|-----------|---------------------|------------|
| `filter` | El **elemento mismo** (su contenido) | Imágenes en gris, desenfoque de un modal de fondo |
| `backdrop-filter` | El **fondo detrás** del elemento | Barras translúcidas, tarjetas de cristal (glassmorphism) |

## Mapa de la subsección

- [[01 filter]] — los efectos sobre el elemento (`blur`, `brightness`, `grayscale`…).
- [[02 backdrop-filter]] — el efecto de cristal esmerilado sobre el fondo.

## Rendimiento: cuidado con el blur

> [!warning] Los filtros (sobre todo blur) son caros
> `blur()` y los filtros en general tienen un **coste de composición** notable, que crece con el tamaño del elemento y se dispara al **animarlos**. `backdrop-filter` es especialmente pesado (repinta el fondo en tiempo real). Úsalos con mesura: una barra translúcida fija está bien; veinte tarjetas con `backdrop-filter` animado pueden ir a tirones, sobre todo en móvil.

## Notas relacionadas

- [[01 filter]] — el punto de partida.
- [[02 backdrop-filter]] — el efecto de cristal, muy de moda.
- [[03 Modos de Mezcla (mix-blend-mode, background-blend-mode)]] — otra forma de manipular color por composición.
