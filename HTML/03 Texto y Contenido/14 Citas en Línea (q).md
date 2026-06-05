---
title: <q> — Cita corta en línea
aliases:
  - q
  - inline quotation
tags:
  - html
  - api/elemento
  - semantica
elemento: q
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
---

# Citas en Línea (q)

> [!definicion]
> `<q>` marca una **cita corta** dentro de una frase. El navegador **añade las comillas
> automáticamente**, adaptándolas al idioma del documento (`«»` en español con `lang="es"`, `""` en
> inglés).

```html
<p>Como dijo Sócrates, <q>solo sé que no sé nada</q>.</p>
```

## q vs. blockquote

| Elemento | Longitud | Comillas | Bloque |
|----------|----------|----------|--------|
| `<q>` | Corta, dentro de una frase | Automáticas | No, en línea |
| [[13 Citas en Bloque (blockquote) | `<blockquote>`]] | Larga, varios párrafos | Manuales (sangrado) | Sí |

## Atributo

| Atributo | Descripción |
|----------|-------------|
| `cite` | URL de la fuente (metadato, no visible) |

> [!warning] No escribir las comillas a mano
> Como `<q>` ya genera las comillas, escribir `"<q>texto</q>"` produce comillas **dobles**. El texto
> va sin comillas: `<q>texto</q>`.

> [!tip] Las comillas siguen al idioma
> Con `lang="es"` el navegador usa `«…»`; con `lang="en"`, `"…"`. Por eso conviene tener bien puesto
> el [[02 Elemento Raíz (html) | `lang`]] del documento: las comillas de las citas dependen de él.

## Notas relacionadas

- [[13 Citas en Bloque (blockquote)]] — para citas largas en bloque.
- [[02 Elemento Raíz (html)]] — el `lang` que decide el estilo de comillas.
