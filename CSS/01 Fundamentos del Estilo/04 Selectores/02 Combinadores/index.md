---
title: Combinadores — Seleccionar por relación entre elementos
aliases:
  - combinators
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Combinadores

> [!definicion]
> Los **combinadores** seleccionan elementos según su **relación** en el árbol DOM con otros: descendientes, hijos directos, hermanos. Conectan dos selectores con un símbolo (espacio, `>`, `+`, `~`) que expresa la relación.

```css
nav a       { }   /* a descendiente de nav */
nav > a     { }   /* a hijo DIRECTO de nav */
h2 + p      { }   /* p inmediatamente tras h2 */
h2 ~ p      { }   /* todo p hermano posterior de h2 */
```

## Los cuatro combinadores

| Combinador | Símbolo | Relación |
|------------|---------|----------|
| Descendente | (espacio) | Cualquier nivel de anidación dentro |
| Hijo directo | `>` | Hijo inmediato (un nivel) |
| Hermano adyacente | `+` | El hermano inmediatamente siguiente |
| Hermano general | `~` | Cualquier hermano posterior |

Cada uno en su nota: [[01 Descendente]], [[02 Hijo Directo (>)]], [[03 Hermano Adyacente (+)]] y [[04 Hermano General (~)]].

## Descendente vs. hijo directo

La distinción más importante:

```html
<nav>
  <a>Directo</a>
  <ul>
    <li><a>Anidado</a></li>
  </ul>
</nav>
```
```css
nav a   { }   /* afecta a AMBOS enlaces (descendientes) */
nav > a { }   /* afecta SOLO al primero (hijo directo) */
```

## Solo "hacia abajo" y "hacia adelante"

> [!info] CSS no tiene selector de padre (clásico)
> Los combinadores tradicionales solo van **hacia abajo** (descendientes) o **hacia adelante** (hermanos siguientes). No existe un combinador de "padre" ni de "hermano anterior". La pseudoclase moderna [[03 has()|`:has()`]] **sí** permite seleccionar "hacia arriba" (un elemento que contiene a otro), llenando ese hueco histórico.

## Buenas prácticas

> [!tip] Recomendaciones
> - Prefiere `>` (hijo directo) cuando quieras acotar a un nivel: más predecible que el descendente.
> - Evita cadenas largas de descendientes (`.a .b .c .d`): frágiles y específicas.
> - Los combinadores de hermano (`+`, `~`) son potentes para patrones como "el primer párrafo tras un título".
> - Para seleccionar hacia arriba, usa `:has()`.

## Notas relacionadas

- [[01 Descendente]] · [[02 Hijo Directo (>)]] — la distinción clave.
- [[03 has()]] — el "selector de padre" moderno.
- [[01 Cálculo de Especificidad]] — combinar no cambia la especificidad (suma la de cada parte).
