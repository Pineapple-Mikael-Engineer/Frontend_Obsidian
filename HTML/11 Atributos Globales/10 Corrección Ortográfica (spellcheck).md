---
title: spellcheck — Corrección ortográfica
aliases:
  - spellcheck
tags:
  - html
  - api/atributo
  - atributos
elemento: global
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
---

# Corrección Ortográfica (spellcheck)

> [!definicion]
> `spellcheck` indica si el navegador debe **revisar la ortografía** del texto que el usuario escribe en un campo editable, subrayando los posibles errores. Se aplica a [[01 input text | campos de texto]], [[03 Área de Texto (textarea) | `<textarea>`]] y elementos [[09 Editable (contenteditable) | `contenteditable`]].

```html
<textarea spellcheck="true">…</textarea>
<input type="text" spellcheck="false" />
```

## Valores

| Valor | Efecto |
|-------|--------|
| `true` | Activa la corrección ortográfica |
| `false` | La desactiva |
| (sin atributo) | El navegador decide (suele depender del tipo de campo) |

## Cuándo desactivarlo

> [!info] No todo campo se beneficia del corrector
> La corrección ortográfica ayuda en texto en prosa (un comentario, un mensaje), pero **estorba** en campos donde el contenido no es lenguaje natural:
> | Campo | `spellcheck` recomendado |
> |-------|--------------------------|
> | Comentario, mensaje, biografía | `true` |
> | Nombre de usuario, email | `false` (no son palabras) |
> | Código, identificadores, claves | `false` |
> | Nombres propios poco comunes | `false` (se marcan como errores) |
>
> Subrayar en rojo un nombre de usuario o un fragmento de código es ruido visual molesto e inútil.

## Depende del idioma

El corrector usa el diccionario del idioma, que toma del [[04 Idioma y Dirección (lang, dir) | `lang`]] del documento o del elemento. Por eso un `lang` correcto también mejora la corrección: con `lang="es"` revisa contra el español; con un `lang` equivocado, marca todo el texto como erróneo.

## Buenas prácticas

> [!tip] Recomendaciones
> - `spellcheck="true"` en campos de prosa (comentarios, descripciones).
> - `spellcheck="false"` en correos, usuarios, código e identificadores.
> - Asegura un `lang` correcto para que el corrector use el diccionario adecuado.

## Errores comunes

> [!warning] Trampas
> - **Corrector en campos no textuales** (email, código): subrayado molesto.
> - **`lang` incorrecto**: el corrector marca todo el texto como error.
> - **Asumir que está siempre activo**: depende del navegador y del campo si no se declara.

## Notas relacionadas

- [[09 Editable (contenteditable)]] — campos editables donde aplica.
- [[04 Idioma y Dirección (lang, dir)]] — el `lang` que decide el diccionario.
- [[01 input text]] — los campos de formulario afectados.
