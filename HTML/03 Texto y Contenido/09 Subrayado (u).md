---
title: <u> — Anotación no textual (subrayado)
aliases:
  - u
  - underline
tags:
  - html
  - api/elemento
  - semantica
elemento: u
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
---

# Subrayado (u)

> [!definicion]
> `<u>` marca un fragmento con una **anotación no textual** que el navegador rinde como subrayado:
> típicamente un error ortográfico señalado o un nombre propio en escritura china (proper name mark).
> No es para subrayar por estética.

```html
<p>Has escrito <u>recibí</u> con tilde incorrecta.</p>
```

> [!warning] El subrayado se confunde con enlace
> En la web, el texto subrayado significa "enlace". Usar `<u>` para resaltar puede hacer que el
> usuario intente hacer clic. Por eso su uso es muy limitado; casi siempre hay una opción mejor.

> [!tip] Alternativas según la intención
> | Intención | Usar |
> |-----------|------|
> | Énfasis | [[05 Énfasis (em) | `<em>`]] |
> | Importancia | [[04 Énfasis Fuerte (strong) | `<strong>`]] |
> | Resaltar relevancia | [[12 Texto Resaltado (mark) | `<mark>`]] |
> | Subrayado puramente decorativo | `text-decoration: underline` en CSS |
>
> `<u>` solo cuando la anotación es genuinamente no textual (error, nombre propio en CJK).

## Notas relacionadas

- [[10 Tachado (s, del)]] — la anotación de "incorrecto/eliminado".
- [[12 Texto Resaltado (mark)]] — para resaltar por relevancia.
