---
title: Agrupación de Selectores — Un bloque para varios selectores
aliases:
  - selector grouping
  - selector list
tags:
  - css
  - api/selector
  - arquitectura
selector: agrupacion
draft: false
---

# Agrupación de Selectores

> [!definicion]
> La **agrupación** aplica un mismo bloque de declaraciones a **varios selectores** a la vez, separándolos por **comas**. Evita repetir el mismo bloque y mantiene el CSS DRY (sin repetición).

```css
h1, h2, h3 {
  font-family: Georgia, serif;
  color: #1e1e2e;
}
```

Equivale a tres reglas idénticas para `h1`, `h2` y `h3`.

## La coma significa "o"

Cada selector de la lista se evalúa **independientemente**: la regla afecta a los elementos que coincidan con `h1` **o** con `h2` **o** con `h3`. Los selectores pueden ser de cualquier tipo y complejidad:

```css
.boton, a.enlace-boton, input[type="submit"] {
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
}
```

## El riesgo del selector inválido

> [!warning] Un selector inválido tumbaba toda la lista
> Históricamente, si **uno** de los selectores de una lista separada por comas era **inválido** (sintaxis no reconocida por el navegador), se descartaba **toda** la regla, incluidos los selectores válidos:
> ```css
> /* Si :nuevo-selector no se reconoce, h1 y h2 TAMBIÉN pierden el estilo */
> h1, h2, :nuevo-selector { color: navy; }
> ```
> La solución moderna es [[02 where()|`:where()`]] o [[01 is()|`:is()`]], que **no** invalidan toda la lista si un selector interno falla. Conviene saberlo al usar selectores muy nuevos.

## Agrupar no es combinar

> [!info] Coma vs. espacio/combinadores
> No confundir agrupar (coma) con combinar (espacio, `>`, etc.):
> | Sintaxis | Significado |
> |----------|-------------|
> | `a, b` | Elementos `a` **o** elementos `b` (agrupación) |
> | `a b` | Elementos `b` **dentro de** `a` (descendiente) |
> | `a > b` | Elementos `b` **hijos directos de** `a` |
>
> La coma une reglas; el espacio expresa relación.

## Buenas prácticas

> [!tip] Recomendaciones
> - Agrupa selectores que comparten estilos para no repetir bloques.
> - No fuerces agrupaciones que junten cosas no relacionadas (dificulta el mantenimiento).
> - Con selectores muy nuevos, considera `:is()`/`:where()` para que un fallo no tumbe la lista.
> - Una línea por selector en listas largas, para legibilidad.

## Errores comunes

> [!warning] Trampas
> - **Confundir coma con espacio**: cambian por completo el significado.
> - **Un selector inválido** en la lista que invalida los demás (en navegadores sin `:is`/`:where`).

## Notas relacionadas

- [[03 Regla Completa]] — varios selectores en una regla.
- [[01 is()]] · [[02 where()]] — agrupación tolerante a fallos.
- [[02 Combinadores/index]] — relacionar elementos (no agrupar).
