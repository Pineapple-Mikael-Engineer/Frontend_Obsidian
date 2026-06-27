---
title: <i> — Cursiva por convención, sin énfasis
aliases:
  - i
  - italic
tags:
  - html
  - api/elemento
  - semantica
elemento: i
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
order: 7
---

# Cursiva sin Énfasis (i)

> [!definicion]
> `<i>` marca texto que se presenta en **cursiva por convención** —no por énfasis—: términos en otro idioma, nombres científicos, títulos de obras, pensamientos, nombres técnicos en su primera aparición. Es la cursiva "tipográfica" que la tradición editorial exige sin que haya recalque vocal.

```html
<p>El término <i lang="la">in vitro</i> significa "en vidrio".</p>
<p>Leí <i>Cien años de soledad</i> el verano pasado.</p>
```

## Convenciones tipográficas que cubre

| Caso | Ejemplo |
|------|---------|
| Idioma extranjero | <i>déjà vu</i>, <i>kawaii</i>, <i>savoir-faire</i> |
| Nombre científico (taxonomía) | <i>Canis lupus</i>, <i>Homo sapiens</i> |
| Título de obra | <i>Don Quijote</i>, <i>El Quijote</i> |
| Pensamiento / voz interior | <i>¿y si no funciona?</i>, pensó |
| Término técnico en su primera mención | el <i>frontend</i> de la aplicación |

## Acompañar con lang en texto extranjero

> [!tip] Combina i con lang
> Para texto en otro idioma, añade el atributo `lang`: así el lector de pantalla lo **pronuncia correctamente** y el corrector ortográfico no lo marca como error.
> ```html
> <p>Pedimos un <i lang="fr">croissant</i> y un <i lang="it">cappuccino</i>.</p>
> ```
> Sin `lang`, la asistencia leerá "croissant" con fonética del idioma de la página.

## i vs. em: la frontera

La diferencia con [[05 Énfasis (em) | `<em>`]] es la **voz**: `<em>` se lee con énfasis (cambia el matiz de la frase); `<i>` se lee normal, solo se ve en cursiva por costumbre. Un nombre científico no se "recalca" al hablar: se escribe en cursiva por convención. Por eso es `<i>`, no `<em>`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<i>` para las convenciones tipográficas (idiomas, taxonomía, títulos, pensamientos).
> - Añade `lang` cuando el texto sea de otro idioma.
> - Para cursiva puramente decorativa de diseño (sin convención detrás), usa `font-style: italic` en CSS.
> - Para énfasis de tono, usa `<em>`.

## Errores comunes

> [!warning] i como sinónimo de cursiva genérica
> Antes de HTML5, `<i>` significaba literalmente "cursiva". Hoy tiene un significado semántico acotado (texto con voz/calidad alternativa por convención). Usarlo para cualquier cursiva estética funciona visualmente pero pierde precisión; cuando la cursiva responde solo al diseño, CSS es la herramienta correcta.

## Notas relacionadas

- [[05 Énfasis (em)]] — la cursiva **con** énfasis semántico.
- [[06 Negrita sin Énfasis (b)]] — el equivalente en negrita sin importancia.
- [[04 Idioma y Dirección (lang, dir)]] — el atributo `lang` para fragmentos en otro idioma.
