---
title: <var> — Variable matemática o de programación
aliases:
  - var
  - variable
tags:
  - html
  - api/elemento
  - semantica
elemento: var
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
---

# Variable (var)

> [!definicion]
> `<var>` marca una **variable**: un nombre de variable en una expresión matemática o de
> programación, o un marcador de posición que el usuario debe sustituir. Se rinde en cursiva por
> defecto.

```html
<p>El área del rectángulo es <var>base</var> × <var>altura</var>.</p>
<p>Ejecuta <code>git clone <var>url-del-repo</var></code>.</p>
```

## var dentro del grupo técnico

| Elemento | Representa |
|----------|------------|
| [[17 Código (code) | `<code>`]] | Código fuente literal |
| `<var>` | Una variable o marcador de posición |
| [[20 Salida de Programa (samp) | `<samp>`]] | Salida de un programa |
| [[21 Entrada de Teclado (kbd) | `<kbd>`]] | Entrada del usuario por teclado |

> [!tip] Combinar con code
> En una orden de terminal, lo fijo va en `<code>` y la parte que el usuario cambia en `<var>`:
> `<code>cd <var>directorio</var></code>`. Distingue lo literal de lo sustituible.

> [!warning] No es para cualquier cursiva
> La cursiva de `<var>` es semántica (es una variable), no estética. Para nombres científicos o
> títulos va [[07 Cursiva sin Énfasis (i) | `<i>`]]; para énfasis, [[05 Énfasis (em) | `<em>`]].

## Notas relacionadas

- [[17 Código (code)]] — lo literal frente a la variable.
- [[20 Salida de Programa (samp)]], [[21 Entrada de Teclado (kbd)]] — el resto del grupo técnico.
