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
> `<em>` marca un **énfasis de entonación** que cambia el sentido de la frase, como cuando se recalca una palabra al hablar. El navegador lo muestra en **cursiva** por defecto, pero lo que comunica es un cambio de voz, no un estilo tipográfico.

```html
<p>No dije que <em>tú</em> tuvieras la culpa.</p>
```

## El énfasis cambia el significado

A diferencia de la negrita o la cursiva decorativas, `<em>` altera lo que la frase comunica. La misma oración cambia de sentido según dónde caiga el énfasis:

```html
<p>No dije que <em>tú</em> tuvieras la culpa.</p>   <!-- la tiene otro -->
<p>No dije que tú tuvieras la <em>culpa</em>.</p>   <!-- fue otra cosa -->
<p>No <em>dije</em> que tú tuvieras la culpa.</p>   <!-- lo insinué, no lo dije -->
```

Esa capacidad de redirigir el matiz es exactamente la función de `<em>`.

## em vs. i

| Elemento | Significado |
|----------|-------------|
| `<em>` | Énfasis hablado: recalca, cambia el matiz de la frase |
| `<i>` | Cursiva por convención: nombres científicos, términos extranjeros, títulos, pensamientos |

Ambos producen cursiva. La diferencia: `<em>` se **lee distinto** (con énfasis); [[07 Cursiva sin Énfasis (i) | `<i>`]] solo se **ve** en cursiva por costumbre tipográfica.

## El énfasis se acumula

`<em>` dentro de `<em>` incrementa el grado de énfasis, igual que ocurre con [[04 Énfasis Fuerte (strong) | `<strong>`]]:

```html
<p><em>De verdad <em>necesito</em> que vengas.</em></p>
```

## Accesibilidad

> [!info] Cómo lo trata la asistencia
> Algunos lectores de pantalla modulan la voz al encontrar `<em>`, reproduciendo el énfasis hablado. Por eso importa reservarlo para cambios reales de entonación: un `<em>` mal puesto introduce un énfasis vocal donde no lo hay.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<em>` cuando, leído en voz alta, ese fragmento se recalcaría.
> - Para cursiva por convención (un *Homo sapiens*, un título de libro), usa `<i>`.
> - Para cursiva puramente estética de diseño, usa `font-style: italic` en CSS.

## Errores comunes

> [!warning] Cursiva ≠ énfasis
> Un nombre científico (*Homo sapiens*) o un término extranjero (*déjà vu*) van en cursiva por convención tipográfica, **no por énfasis**: eso es `<i>`, no `<em>`. Usar `<em>` para toda cursiva mete énfasis vocal donde no corresponde y empobrece la lectura asistida.

## Notas relacionadas

- [[04 Énfasis Fuerte (strong)]] — la importancia (negrita), su pareja semántica.
- [[07 Cursiva sin Énfasis (i)]] — la cursiva por convención, sin énfasis.
