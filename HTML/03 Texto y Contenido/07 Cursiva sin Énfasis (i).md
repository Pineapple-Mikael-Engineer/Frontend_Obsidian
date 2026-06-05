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
---

# Cursiva sin Énfasis (i)

> [!definicion]
> `<i>` marca texto que se presenta en **cursiva por convención** —no por énfasis—: términos en otro
> idioma, nombres científicos, títulos de obras, pensamientos, términos técnicos en su primera
> aparición.

```html
<p>El término <i lang="la">in vitro</i> significa "en vidrio".</p>
<p>Leí <i>Cien años de soledad</i> el verano pasado.</p>
```

## Convenciones tipográficas que cubre

| Caso | Ejemplo |
|------|---------|
| Idioma extranjero | <i>déjà vu</i>, <i>kawaii</i> |
| Nombre científico | <i>Canis lupus</i> |
| Título de obra | <i>Don Quijote</i> |
| Pensamiento / voz interior | <i>¿y si no funciona?</i>, pensó |

> [!tip] Acompañar con lang
> Para texto en otro idioma, añadir el atributo `lang`: `<i lang="fr">croissant</i>`. Así el lector
> de pantalla lo pronuncia correctamente y el corrector no lo marca como error.

> [!warning] No es énfasis
> La cursiva de `<i>` es convención, no recalque. Si se quiere **enfatizar el tono** de una palabra,
> es [[05 Énfasis (em) | `<em>`]]. Si solo es estilo de marca, una clase CSS con `font-style`.

## Notas relacionadas

- [[05 Énfasis (em)]] — la cursiva **con** énfasis semántico.
- [[06 Negrita sin Énfasis (b)]] — el equivalente en negrita sin importancia.
