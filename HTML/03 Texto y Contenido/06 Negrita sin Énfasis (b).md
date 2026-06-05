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
> `<b>` aplica **negrita** para llamar la atención sobre un texto **sin** atribuirle mayor importancia ni énfasis: palabras clave en un resumen, nombres de producto, la frase inicial de un artículo, términos destacados en una guía. Es estilística, no semántica.

```html
<p>Mezcle la <b>harina</b> con el <b>azúcar</b> antes de añadir los huevos.</p>
```

## Cuándo usar b (y cuándo no)

| Caso | Elemento |
|------|----------|
| Resaltar palabras clave sin darles más peso | `<b>` |
| Texto realmente importante o urgente | `<strong>` |
| Énfasis de entonación | `<em>` |
| Negrita ligada al estilo de marca | clase CSS con `font-weight` |

Para importancia real se usa [[04 Énfasis Fuerte (strong) | `<strong>`]]; para énfasis de tono, [[05 Énfasis (em) | `<em>`]]. `<b>` es el que queda cuando solo se quiere destacar a la vista.

## El "último recurso" semántico

> [!info] Cuándo lo recomienda la especificación
> La especificación describe `<b>` como el elemento a usar **cuando ningún otro encaja**: no es importancia (`<strong>`), ni énfasis (`<em>`), ni término técnico (`<code>`), ni relevancia contextual ([[12 Texto Resaltado (mark) | `<mark>`]])… solo "destacar visualmente sin connotación". Si existe un elemento más específico, se prefiere ese. `<b>` es la opción por defecto para negrita neutra.

## Ejemplos en contexto

```html
<!-- Frase introductoria destacada (lead) -->
<p><b>Madrid, 4 jun.</b> El ayuntamiento anunció hoy…</p>

<!-- Nombres de producto en una reseña -->
<p>Comparamos el <b>Modelo A</b> con el <b>Modelo B</b>.</p>

<!-- Palabras clave en una guía de pasos -->
<p>Pulse <b>Guardar</b> y luego <b>Salir</b>.</p>
```

## Accesibilidad

> [!info] Voz neutra
> Un lector de pantalla lee `<b>` con **voz normal**, sin enfatizar. Si el texto debe percibirse como importante también sin verlo (un usuario ciego), entonces no es `<b>`: es `<strong>`. `<b>` solo afecta a la vista.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<b>` para negrita neutra cuando ningún elemento semántico aplica.
> - Si la negrita responde a un sistema de diseño (todos los "leads" en negrita), considera una clase CSS en su lugar, para separar estilo de contenido.
> - No lo uses para importancia (`<strong>`) ni énfasis (`<em>`).

## Errores comunes

> [!warning] Confundir b con strong
> Como ambos dan negrita, es fácil usarlos al azar. Pregúntate: *¿este texto es más importante, o solo más visible?* Importante → `<strong>`; visible → `<b>`. Usar siempre uno de los dos "por costumbre" pierde el matiz que la asistencia y el SEO sí aprovechan.

## Notas relacionadas

- [[04 Énfasis Fuerte (strong)]] — cuando el texto **sí** es importante.
- [[07 Cursiva sin Énfasis (i)]] — el equivalente en cursiva sin énfasis.
