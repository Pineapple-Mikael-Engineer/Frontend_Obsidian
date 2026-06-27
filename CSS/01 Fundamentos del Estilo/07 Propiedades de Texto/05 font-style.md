---
title: font-style — Cursiva y oblicua
aliases:
  - font-style
  - italic
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: font-style
grupo: tipografia
valor_inicial: normal
hereda: true
animable: false
draft: false
order: 5
---

# font-style

> [!definicion]
> `font-style` controla si el texto se muestra **normal**, en **cursiva** (italic) o en **oblicua** (oblique). La cursiva e la oblicua difieren en cómo se generan: la cursiva usa un diseño caligráfico aparte; la oblicua, una simple inclinación del normal.

```css
em        { font-style: italic; }
.cita     { font-style: oblique; }
.no-cursi { font-style: normal; }
```

## Valores

| Valor | Efecto |
|-------|--------|
| `normal` | Texto recto (por defecto) |
| `italic` | Cursiva: usa el diseño *italic* de la fuente si existe |
| `oblique` | Oblicua: inclina el diseño normal |
| `oblique <ángulo>` | Inclina un ángulo concreto (`oblique 14deg`) |

## italic vs. oblique

> [!info] Dos formas de "inclinar"
> No son lo mismo:
> - **`italic`**: pide la variante *italic* de la fuente, que suele ser un **diseño distinto** (letras con formas caligráficas, no solo inclinadas). Si la fuente no tiene italic, el navegador la **sintetiza** inclinando el normal.
> - **`oblique`**: simplemente **inclina** el diseño normal un ángulo, sin cambiar las formas.
>
> Para la mayoría de fuentes de texto con un italic real, `italic` da mejor resultado tipográfico. `oblique <ángulo>` es útil con fuentes variables que tienen un eje de inclinación.

## La cursiva sintética

Si pides `italic` y la fuente carece de variante italic, el navegador **falsea** la cursiva inclinando las letras. El resultado es aceptable pero inferior a un italic diseñado. Para textos donde la tipografía importa, conviene **cargar** la variante italic de la fuente.

## Relación con la semántica HTML

`font-style: italic` es la presentación; la **semántica** de poner algo en cursiva la dan los elementos HTML [[05 Énfasis (em) | `<em>`]] (énfasis) e [[07 Cursiva sin Énfasis (i) | `<i>`]] (convención). CSS controla el aspecto; el elemento, el significado. Para "quitar" la cursiva de un `<em>` (raro), `font-style: normal`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `italic` para cursiva de texto; da mejor resultado que `oblique` en fuentes con italic real.
> - Carga la variante italic de tu fuente si la usas mucho (evita la cursiva sintética).
> - `font-style: normal` para anular la cursiva por defecto de `<em>`/`<address>` cuando el diseño lo pida.
> - `oblique <ángulo>` con fuentes variables que tengan eje de inclinación.

## Errores comunes

> [!warning] Trampas
> - **Cursiva sintética** de baja calidad por no cargar la variante italic.
> - **Confundir presentación con semántica**: la cursiva visual no implica énfasis.

## Notas relacionadas

- [[04 font-weight]] — el grosor, la otra dimensión del estilo.
- [[05 Énfasis (em)]] · [[07 Cursiva sin Énfasis (i)]] — la semántica de la cursiva.
- [[03 font-variation-settings]] — el eje de inclinación de fuentes variables.
