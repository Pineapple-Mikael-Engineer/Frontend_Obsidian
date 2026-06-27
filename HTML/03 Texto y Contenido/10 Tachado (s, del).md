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
order: 10
---

# Tachado (s, del)

> [!definicion]
> Ambos se rinden **tachados**, pero significan cosas distintas. `<s>` marca texto que ya **no es relevante o exacto** (un precio antiguo). `<del>` marca contenido **eliminado en una edición** del documento, y tiene como contraparte a [[11 Texto Insertado (ins) | `<ins>`]] para lo añadido.

```html
<p>Antes <s>49,99 €</s>, ahora <strong>29,99 €</strong>.</p>

<p>La reunión es el <del>lunes</del> <ins>martes</ins>.</p>
```

## s vs. del: el criterio

| Elemento | Significado | Rol ARIA |
|----------|-------------|----------|
| `<s>` | Ya no es preciso ni relevante (no es una edición del documento) | ninguno |
| `<del>` | Eliminado en una revisión/edición del documento | `deletion` |

La pregunta: *¿estoy editando el documento (mostrando un cambio), o el texto simplemente dejó de aplicar?* Un precio rebajado es `<s>` (ya no aplica, pero nadie "editó" el documento). Un cambio en un contrato versionado es `<del>` (se eliminó del texto). El estilo coincide; la semántica no.

## Atributos de del (e ins)

| Atributo | Descripción |
|----------|-------------|
| `cite` | URL que documenta el motivo del cambio |
| `datetime` | Momento de la edición, en formato ISO 8601 |

```html
<p>El plazo es de <del datetime="2026-06-01" cite="/changelog">30</del>
   <ins datetime="2026-06-01">45</ins> días.</p>
```

## Accesibilidad

> [!warning] El tachado es casi invisible para la asistencia
> Por defecto, un lector de pantalla **no anuncia** "tachado" ni "eliminado" al encontrar `<s>` o `<del>`: lee el texto como cualquier otro. Si la tacha es crítica para entender el contenido (un precio que ya no aplica, una cláusula eliminada), no basta con el estilo visual: refuérzalo con texto explícito ("precio anterior:", "eliminado:") para no dejar fuera a quien no ve la pantalla.

## Buenas prácticas

> [!tip] Recomendaciones
> - Precio/dato que dejó de aplicar → `<s>`. Cambio en un documento versionado → `<del>` (con su `<ins>`).
> - En `<del>`/`<ins>`, añade `datetime` y, si procede, `cite` para documentar la edición.
> - No confíes solo en el tachado visual para información esencial; acompáñalo de texto.

## Errores comunes

> [!warning] Intercambiar s y del
> Usar `<del>` para un precio rebajado (no hubo edición del documento) o `<s>` para un cambio versionado invierte el significado. Aunque visualmente sea idéntico, las herramientas que distinguen ediciones (control de cambios, diffs) se basan en `<del>`/`<ins>`.

## Notas relacionadas

- [[11 Texto Insertado (ins)]] — la contraparte de `<del>`: contenido añadido.
- [[12 Texto Resaltado (mark)]] — otro marcado contextual del texto.
