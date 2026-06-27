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
order: 11
---

# Texto Insertado (ins)

> [!definicion]
> `<ins>` marca contenido **añadido** en una revisión del documento. Es la contraparte de [[10 Tachado (s, del) | `<del>`]]: juntos representan los cambios de una edición (lo que se quitó y lo que se puso). El navegador lo muestra **subrayado** por defecto.

```html
<p>La reunión es el <del>lunes</del> <ins>martes</ins> a las 10:00.</p>
```

## Atributos

| Atributo | Descripción |
|----------|-------------|
| `cite` | URL que documenta el motivo del cambio |
| `datetime` | Momento de la inserción, en formato ISO 8601 (`2026-06-04T10:00`) |

```html
<p>Capacidad máxima: <ins datetime="2026-06-04" cite="/acta">120</ins> personas.</p>
```

## Pareja con del

`<ins>` rara vez aparece solo: su sentido pleno surge junto a `<del>`, mostrando "esto se quitó, esto se puso". Es la base semántica del control de cambios:

```html
<p>Precio: <del>30 €</del> <ins>25 €</ins> (rebaja aplicada).</p>
```

Para resaltar relevancia **sin** que sea una edición del documento, el elemento correcto es [[12 Texto Resaltado (mark) | `<mark>`]], no `<ins>`.

## Accesibilidad

> [!warning] La inserción no se anuncia por defecto
> Igual que con `<del>`, la mayoría de lectores de pantalla **no anuncian** "insertado" al leer `<ins>`: lo leen normal. Si el hecho de ser una adición es importante para comprender el contenido, conviene reforzarlo con texto o con CSS que lo distinga claramente.

## Estilo: el subrayado parece enlace

El subrayado por defecto de `<ins>` puede confundirse con un enlace (mismo problema que [[09 Subrayado (u) | `<u>`]]). En documentos con seguimiento de cambios se reestila para diferenciarlo:

```css
ins { text-decoration: none; background: #2e4d2e; color: #a6e3a1; }
del { color: #f38ba8; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<ins>` emparejado con `<del>` para representar ediciones reales.
> - Añade `datetime` (y `cite` si procede) para fechar el cambio.
> - Reestila el subrayado por defecto si puede confundirse con enlaces.
> - Para resaltar sin que sea edición, usa `<mark>`.

## Errores comunes

> [!warning] ins para resaltar
> Usar `<ins>` solo "para subrayar y destacar" malinterpreta su semántica (es una edición del documento) y, además, genera el subrayado que parece enlace. El resaltado contextual es `<mark>`; la importancia, `<strong>`.

## Notas relacionadas

- [[10 Tachado (s, del)]] — la contraparte: contenido eliminado.
- [[09 Subrayado (u)]] — otro elemento que se rinde subrayado y se confunde con enlaces.
- [[12 Texto Resaltado (mark)]] — para resaltar sin que sea una edición.
