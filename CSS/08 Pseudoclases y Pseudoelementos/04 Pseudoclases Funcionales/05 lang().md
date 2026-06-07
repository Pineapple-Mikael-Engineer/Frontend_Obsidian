---
title: lang() — Seleccionar por idioma
aliases:
  - ":lang"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":lang"
grupo: pseudoclase
draft: false
---

# :lang()

> [!definicion]
> `:lang(código)` selecciona elementos según el **idioma** de su contenido, determinado por el atributo `lang` del HTML. Permite aplicar estilos tipográficos específicos de cada idioma: comillas, separación de palabras, fuentes apropiadas.

```css
:lang(fr) { quotes: "«" "»"; }   /* comillas francesas */
```

## Estilos tipográficos por idioma

> [!tip] Cada idioma tiene sus convenciones
> El uso principal: aplicar las convenciones tipográficas correctas según el idioma. Las **comillas** son el ejemplo clásico (cada idioma usa las suyas):
> ```css
> :lang(es) { quotes: "«" "»" "“" "”"; }   /* español: comillas latinas */
> :lang(en) { quotes: "“" "”" "‘" "’"; }   /* inglés: comillas inglesas */
> :lang(fr) { quotes: "« " " »"; }          /* francés: con espacios */
> :lang(de) { quotes: "„" "“"; }            /* alemán: comillas bajas */
> ```
> Las comillas las inserta el pseudoelemento [[01 before y after | `::before`/`::after`]] con [[02 Propiedad content | `content: open-quote`]], respetando el idioma.

## Cómo se determina el idioma

`:lang()` mira el atributo `lang` del elemento o de un **ancestro** (se hereda por el árbol):

```html
<html lang="es">
  <p>Texto en español</p>
  <blockquote lang="fr">Texte en français</blockquote>
</html>
```

```css
:lang(es) { /* aplica al <p> (hereda de <html lang="es">) */ }
:lang(fr) { /* aplica al <blockquote lang="fr"> */ }
```

## :lang() vs. [lang|=]

> [!info] :lang() entiende los subcódigos
> Hay una diferencia con el selector de atributo `[lang="..."]`:
> - **`[lang="en"]`** coincide solo con `lang="en"` exacto.
> - **`:lang(en)`** coincide con `en`, `en-US`, `en-GB`… (entiende la **jerarquía** de códigos de idioma).
>
> `:lang(en)` es más correcto para "cualquier variante de inglés". Para eso con atributos haría falta `[lang|="en"]` (que coincide con `en` y `en-*`).

## Otros usos por idioma

```css
/* Fuente apropiada para cada escritura */
:lang(ja) { font-family: "Noto Sans JP", sans-serif; }
:lang(ar) { font-family: "Noto Sans Arabic", sans-serif; direction: rtl; }

/* Separación de palabras según el idioma */
:lang(de) { hyphens: auto; }   /* alemán: palabras largas */
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Declara el `lang` en el HTML (`<html lang="es">` y en fragmentos de otro idioma).
> - Usa `:lang()` para comillas, fuentes y separación específicas de cada idioma.
> - Prefiere `:lang(en)` a `[lang="en"]` para cubrir las variantes (`en-US`, `en-GB`).
> - Combínalo con `quotes` + `content: open-quote` para comillas correctas.

## Errores comunes

> [!warning] Trampas
> - **Sin `lang` en el HTML**: `:lang()` no tiene de dónde determinar el idioma.
> - **Usar `[lang="en"]`** y no cubrir `en-US`: usa `:lang(en)`.
> - **Comillas hardcodeadas** en vez de respetar el idioma con `quotes`.

## Notas relacionadas

- [[02 Propiedad content]] — `open-quote`/`close-quote` para las comillas.
- [[06 Escritura y Dirección/index]] — `direction: rtl` para idiomas RTL.
- [[01 Idioma (lang)]] — el atributo `lang` desde HTML.
