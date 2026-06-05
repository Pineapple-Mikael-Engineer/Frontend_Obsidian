---
title: <s> y <del> — Texto tachado e información eliminada
aliases:
  - s
  - del
  - strikethrough
tags:
  - html
  - api/elemento
  - semantica
elemento: del
categoria: fraseo
rol_implicito: deletion
vacio: false
draft: false
---

# Tachado (s, del)

> [!definicion]
> Ambos se rinden **tachados**, pero significan cosas distintas. `<s>` marca texto que ya **no es
> relevante o exacto** (un precio antiguo). `<del>` marca contenido **eliminado en una edición**, y
> tiene su contraparte [[11 Texto Insertado (ins) | `<ins>`]] para lo añadido.

```html
<p>Antes <s>49,99 €</s> ahora <strong>29,99 €</strong>.</p>

<p>La reunión es el <del>lunes</del> <ins>martes</ins>.</p>
```

## s vs. del

| Elemento | Significado | Rol ARIA |
|----------|-------------|----------|
| `<s>` | Ya no es preciso/relevante (no es una edición) | ninguno |
| `<del>` | Eliminado en una revisión del documento | `deletion` |

## Atributos de `<del>`

| Atributo | Descripción |
|----------|-------------|
| `cite` | URL que explica el motivo del cambio |
| `datetime` | Momento de la edición (formato ISO 8601) |

> [!info] Accesibilidad del tachado
> El tachado es **invisible** para un lector de pantalla salvo configuración especial: no anuncia
> "tachado" por defecto. Si la eliminación es crítica para entender el contenido, conviene reforzarla
> con texto ("precio anterior:") y no confiar solo en el estilo.

> [!warning] No confundir significados
> Un precio rebajado es `<s>` (ya no aplica), no `<del>` (no se eliminó del documento). Un cambio en
> un documento versionado es `<del>`/`<ins>`. El estilo coincide; la semántica no.

## Notas relacionadas

- [[11 Texto Insertado (ins)]] — la contraparte: contenido añadido.
- [[12 Texto Resaltado (mark)]] — otro marcado contextual del texto.
