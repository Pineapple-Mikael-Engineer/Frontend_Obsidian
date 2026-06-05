---
title: <ins> — Texto insertado en una edición
aliases:
  - ins
  - inserted text
tags:
  - html
  - api/elemento
  - semantica
elemento: ins
categoria: fraseo
rol_implicito: insertion
vacio: false
draft: false
---

# Texto Insertado (ins)

> [!definicion]
> `<ins>` marca contenido **añadido** en una revisión del documento. Es la contraparte de
> [[10 Tachado (s, del) | `<del>`]]: juntos representan los cambios de una edición. El navegador lo
> muestra **subrayado** por defecto.

```html
<p>La reunión es el <del>lunes</del> <ins>martes</ins> a las 10:00.</p>
```

## Atributos

| Atributo | Descripción |
|----------|-------------|
| `cite` | URL que documenta el motivo del cambio |
| `datetime` | Momento de la inserción (ISO 8601: `2026-06-04T10:00`) |

> [!warning] El subrayado de ins parece enlace
> Igual que [[09 Subrayado (u) | `<u>`]], `<ins>` se rinde subrayado y puede confundirse con un
> enlace. En documentos con seguimiento de cambios suele re-estilarse (color verde, fondo) para
> distinguir inserciones de enlaces.

> [!info] Pareja con del
> `<ins>` rara vez aparece solo: su sentido pleno surge junto a `<del>` para mostrar "esto se quitó,
> esto se puso". Para resaltar relevancia sin que sea una edición, usar
> [[12 Texto Resaltado (mark) | `<mark>`]].

## Notas relacionadas

- [[10 Tachado (s, del)]] — la contraparte: contenido eliminado.
- [[09 Subrayado (u)]] — otro elemento que se rinde subrayado.
