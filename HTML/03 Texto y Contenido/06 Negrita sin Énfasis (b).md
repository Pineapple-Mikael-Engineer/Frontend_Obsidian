---
title: <b> — Negrita sin importancia semántica
aliases:
  - b
  - bold
tags:
  - html
  - api/elemento
  - semantica
elemento: b
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
---

# Negrita sin Énfasis (b)

> [!definicion]
> `<b>` aplica **negrita** para llamar la atención sobre un texto **sin** atribuirle mayor
> importancia ni énfasis: palabras clave en un resumen, nombres de producto, la primera frase de un
> artículo. Es estilística, no semántica.

```html
<p>Mezcle la <b>harina</b> con el <b>azúcar</b> antes de añadir los huevos.</p>
```

## Cuándo usar b (y cuándo no)

| Caso | Elemento |
|------|----------|
| Resaltar palabras clave sin importancia | `<b>` |
| Texto realmente importante / urgente | [[04 Énfasis Fuerte (strong) | `<strong>`]] |
| Cualquier negrita ligada a un estilo de marca | clase CSS con `font-weight` |

> [!info] El último recurso semántico
> La especificación describe `<b>` como el elemento a usar **cuando ningún otro encaja**: no es
> importancia (`<strong>`), ni énfasis (`<em>`), ni término técnico, ni resaltado contextual
> ([[12 Texto Resaltado (mark) | `<mark>`]])… solo "destacar visualmente". Si existe un elemento más
> específico, se prefiere.

> [!warning] No comunica nada a la asistencia
> Un lector de pantalla lee `<b>` con voz normal. Si el texto debe percibirse como importante también
> sin verlo, es `<strong>`, no `<b>`.

## Notas relacionadas

- [[04 Énfasis Fuerte (strong)]] — cuando el texto **sí** es importante.
- [[07 Cursiva sin Énfasis (i)]] — el equivalente en cursiva sin énfasis.
