---
title: <em> — Énfasis de tono
aliases:
  - em
  - emphasis
tags:
  - html
  - api/elemento
  - semantica
elemento: em
categoria: fraseo
rol_implicito: emphasis
vacio: false
draft: false
---

# Énfasis (em)

> [!definicion]
> `<em>` marca un **énfasis de entonación** que cambia el sentido de la frase, como cuando se
> recalca una palabra al hablar. El navegador lo muestra en **cursiva** por defecto.

```html
<p>No dije que <em>tú</em> tuvieras la culpa.</p>
```

El énfasis altera el significado: *"no dije que **tú** tuvieras la culpa"* (otro la tiene) es
distinto de *"no dije que tú tuvieras la **culpa**"* (fue otra cosa). Esa es la función de `<em>`.

## em vs. i

| Elemento | Significado |
|----------|-------------|
| `<em>` | Énfasis hablado, cambia el matiz de la frase |
| [[07 Cursiva sin Énfasis (i) | `<i>`]] | Cursiva por convención: nombres científicos, términos extranjeros, títulos |

> [!info] Anidar aumenta el énfasis
> `<em>` dentro de `<em>` incrementa el grado de énfasis. Igual que con
> [[04 Énfasis Fuerte (strong) | `<strong>`]], la semántica se acumula con la anidación.

> [!warning] Cursiva ≠ énfasis
> Un nombre científico (*Homo sapiens*) va en cursiva por convención tipográfica, no por énfasis:
> eso es `<i>`, no `<em>`. Usar `<em>` solo cuando se recalca el tono. La cursiva estética es
> `font-style: italic` en CSS.

## Notas relacionadas

- [[04 Énfasis Fuerte (strong)]] — la importancia (negrita), su pareja semántica.
- [[07 Cursiva sin Énfasis (i)]] — la cursiva por convención, sin énfasis.
