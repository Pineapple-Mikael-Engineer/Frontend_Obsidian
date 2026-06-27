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
order: 14
---

# Citas en Línea (q)

> [!definicion]
> `<q>` marca una **cita corta** dentro de una frase. Su rasgo distintivo: el navegador **añade las comillas automáticamente**, y las adapta al idioma del documento (`«»` en español con `lang="es"`, `""` en inglés).

```html
<p>Como dijo Sócrates, <q>solo sé que no sé nada</q>.</p>
```

## q vs. blockquote

| Elemento | Longitud | Comillas | Tipo |
|----------|----------|----------|------|
| `<q>` | Corta, dentro de una frase | Automáticas, según idioma | En línea |
| `<blockquote>` | Larga, uno o más párrafos | Manuales (sangrado/borde) | Bloque |

Para citas de bloque, ver [[13 Citas en Bloque (blockquote) | `<blockquote>`]].

## Las comillas siguen al idioma

El navegador elige el estilo de comillas según el `lang` del contexto, a través de la propiedad CSS `quotes`:

```html
<p lang="es">Dijo <q>hola</q>.</p>   <!-- «hola» -->
<p lang="en">Said <q>hello</q>.</p>  <!-- "hello" -->
<p lang="fr">Dit <q>bonjour</q>.</p> <!-- « bonjour » -->
```

Por eso conviene tener bien declarado el [[02 Elemento Raíz (html) | `lang`]] del documento: las comillas de las citas dependen de él. El estilo se puede personalizar con la propiedad `quotes` en CSS.

## Atributo

| Atributo | Descripción |
|----------|-------------|
| `cite` | URL de la fuente de la cita (metadato, no visible) |

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<q>` para citas breves integradas en una frase; deja que genere las comillas.
> - Asegúrate de que el `lang` del documento sea correcto para que las comillas sean las del idioma.
> - Para citas largas o de varios párrafos, usa `<blockquote>`.

## Errores comunes

> [!warning] Escribir las comillas a mano
> Como `<q>` ya genera las comillas, escribir `"<q>texto</q>"` produce **comillas dobles**: las del autor más las automáticas. El texto va sin comillas manuales: `<q>texto</q>`. Si por algún motivo no quieres las comillas automáticas, no uses `<q>` (usa texto con comillas escritas), o anúlalas con `q { quotes: none; }` en CSS.

## Notas relacionadas

- [[13 Citas en Bloque (blockquote)]] — para citas largas en bloque.
- [[02 Elemento Raíz (html)]] — el `lang` que decide el estilo de comillas.
