---
title: text-rendering — Pista de prioridad al motor de texto
aliases:
  - text-rendering
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: text-rendering
grupo: tipografia
valor_inicial: auto
hereda: true
animable: false
draft: false
order: 6
---

# text-rendering

> [!definicion]
> `text-rendering` da una **pista** al navegador sobre qué priorizar al dibujar el texto: velocidad, legibilidad o precisión geométrica. No es una propiedad CSS estándar de pleno derecho (viene de SVG), y su efecto varía entre navegadores.

```css
body { text-rendering: optimizeLegibility; }
```

## Valores

| Valor | Prioriza |
|-------|----------|
| `auto` | El navegador decide (por defecto) |
| `optimizeSpeed` | Velocidad: desactiva kerning y ligaduras |
| `optimizeLegibility` | Legibilidad: activa kerning y ligaduras |
| `geometricPrecision` | Precisión geométrica del trazado |

## optimizeLegibility y su coste

> [!warning] optimizeLegibility puede ralentizar
> `optimizeLegibility` activa kerning y ligaduras para un texto más cuidado, pero tiene un **coste de rendimiento**: en páginas con **mucho** texto, puede ralentizar el renderizado y la carga inicial, sobre todo en móviles. Históricamente causó bugs de "texto invisible al cargar". Por eso no conviene aplicarlo globalmente a `body` sin medir; úsalo en elementos concretos (titulares) donde el detalle importe.

## Casi siempre, auto basta

> [!info] Las propiedades específicas son mejores
> En la práctica, `auto` ya activa kerning y ligaduras donde importan. Si quieres control real sobre esas funciones, las propiedades **específicas** son más predecibles y portables que `text-rendering`:
> - Kerning → [[01 font-kerning | `font-kerning`]].
> - Ligaduras → [[06 font-variant | `font-variant-ligatures`]].
>
> `text-rendering` es una pista vaga; estas propiedades son instrucciones concretas.

## Buenas prácticas

> [!tip] Recomendaciones
> - Deja `auto` salvo necesidad concreta; suele ser suficiente.
> - No apliques `optimizeLegibility` global a páginas con mucho texto (coste de rendimiento).
> - Para control real de kerning/ligaduras, usa `font-kerning`/`font-variant-ligatures`.
> - Si lo usas, limítalo a titulares y bloques cortos.

## Errores comunes

> [!warning] Trampas
> - **`optimizeLegibility` en `body`** de páginas largas: ralentiza.
> - **Esperar comportamiento idéntico** entre navegadores: es una pista no estandarizada del todo.
> - **Usarlo en vez de** las propiedades específicas de kerning/ligaduras.

## Notas relacionadas

- [[01 font-kerning]] — control específico del kerning.
- [[06 font-variant]] — control específico de ligaduras.
- [[02 font-feature-settings]] — las features OpenType de bajo nivel.
