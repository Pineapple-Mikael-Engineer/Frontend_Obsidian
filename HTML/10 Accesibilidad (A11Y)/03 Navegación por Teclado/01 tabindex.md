---
title: tabindex — Controlar el orden de tabulación
aliases:
  - tabindex
tags:
  - html
  - api/atributo
  - a11y
elemento: global
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
order: 1
---

# tabindex

> [!definicion]
> `tabindex` controla si un elemento es **enfocable por teclado** y en qué **orden**. Es un atributo global delicado: usado mal, rompe la navegación. La regla práctica se reduce a tres valores —`0`, `-1` y (a evitar) positivos—, cada uno con un propósito muy concreto.

```html
<div tabindex="0" role="button">Enfocable por teclado</div>
<div tabindex="-1">Enfocable solo por JavaScript</div>
```

## Los tres casos

| `tabindex` | Efecto |
|------------|--------|
| `0` | Hace enfocable un elemento que no lo es de forma nativa, en su orden natural del DOM |
| `-1` | Enfocable **solo por JavaScript** (`.focus()`), no con `Tab` |
| positivo (`1`, `2`…) | Fuerza un orden de tabulación explícito — **a evitar** |

## tabindex="0": añadir al orden natural

Hace que un elemento normalmente no enfocable (un `<div>` que actúa como widget) entre en el recorrido de `Tab`, en la posición que le toca por el DOM:

```html
<div role="button" tabindex="0" onclick="…" onkeydown="…">Acción</div>
```

> [!info] Solo para elementos no nativos
> `tabindex="0"` solo hace falta en elementos **sin** interactividad nativa. Un `<button>` o `<a href>` ya es enfocable; añadirle `tabindex="0"` es redundante. Su uso legítimo es dar foco a un componente a medida (un widget ARIA).

## tabindex="-1": foco programático

Saca el elemento del orden de `Tab`, pero permite enfocarlo desde JavaScript con `.focus()`. Útil para mover el foco a una zona tras una acción (a un mensaje de error, al [[04 Contenido Principal (main) | `<main>`]] tras un [[03 Skip Links | skip link]]):

```html
<main id="contenido" tabindex="-1">…</main>
```

```js
document.getElementById("contenido").focus();   // foco tras saltar
```

## tabindex positivo: el antipatrón

> [!warning] Nunca uses tabindex positivo
> Un `tabindex="1"` (o mayor) fuerza ese elemento al **frente** del orden de tabulación, **antes** que todo lo que tiene `tabindex="0"` o foco natural. Esto:
> - Rompe el orden lógico del DOM, creando saltos confusos.
> - Es una pesadilla de mantener (añadir un campo descuadra toda la secuencia).
> - Casi siempre indica un problema de orden que debería resolverse **reordenando el HTML**, no con tabindex.
>
> Si el orden de tabulación no es el correcto, **cambia el orden del marcado**, no metas tabindex positivos.

## Buenas prácticas

> [!tip] Recomendaciones
> - Solo `0` (hacer enfocable) y `-1` (foco solo por JS). Nada de positivos.
> - No pongas `tabindex` en elementos ya enfocables (botones, enlaces).
> - Si el orden de foco está mal, reordena el HTML.
> - Un elemento con `tabindex="0"` y rol de widget **necesita** también manejo de teclado.

## Errores comunes

> [!warning] Trampas
> - **`tabindex` positivo**: rompe el orden natural.
> - **`tabindex="0"` redundante** en botones/enlaces.
> - **Hacer enfocable un `<div>`** sin darle rol ni teclado: el usuario llega a algo "mudo".

## Notas relacionadas

- [[06 Tabulación (tabindex)]] — `tabindex` como atributo global (referencia).
- [[02 Gestión de Foco (focus, blur)]] — mover el foco con JavaScript.
- [[03 Roles de Widget]] — los componentes que necesitan `tabindex="0"`.
