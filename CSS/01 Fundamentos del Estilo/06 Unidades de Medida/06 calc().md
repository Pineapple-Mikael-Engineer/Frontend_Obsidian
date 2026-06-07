---
title: calc() — Cálculos con mezcla de unidades
aliases:
  - calc
tags:
  - css
  - api/funcion
  - arquitectura
draft: false
---

# calc()

> [!definicion]
> La función `calc()` realiza **operaciones aritméticas** dentro de un valor CSS, y su gran poder es **mezclar unidades distintas** que de otro modo no se pueden combinar: `calc(100% - 2rem)`, `calc(50vw + 20px)`. Resuelve el cálculo en tiempo de renderizado, adaptándose al contexto real.

```css
width: calc(100% - 2rem);        /* todo el ancho menos un margen fijo */
height: calc(100vh - 60px);      /* alto de pantalla menos una cabecera */
font-size: calc(1rem + 0.5vw);   /* base + un poco de fluidez */
```

## Operadores

| Operador | Uso | Nota |
|----------|-----|------|
| `+` | Suma | **Requiere espacios** alrededor |
| `-` | Resta | **Requiere espacios** alrededor |
| `*` | Multiplicación | Espacios opcionales |
| `/` | División | Espacios opcionales; divisor sin unidad |

## La regla de los espacios

> [!warning] + y - exigen espacios alrededor
> El error nº 1 con `calc()`: olvidar los espacios en torno a `+` y `-`. Sin ellos, el parser no distingue el operador de un signo o de parte de la unidad, e **invalida** todo el `calc()`:
> ```css
> width: calc(100%-2rem);     /* ❌ inválido: faltan espacios */
> width: calc(100% - 2rem);   /* ✅ */
> ```
> Para `*` y `/` los espacios son opcionales, pero por consistencia conviene ponerlos siempre.

## El caso típico: restar a un porcentaje

El uso más frecuente es restar una cantidad **fija** a una **fluida**, algo imposible sin `calc()`:

```css
/* Una columna que ocupa todo menos una barra lateral fija */
.contenido { width: calc(100% - 250px); }

/* Alto de pantalla descontando una cabecera */
.main { min-height: calc(100vh - var(--altura-header)); }
```

## Anidar y usar variables

`calc()` puede contener [[01 Definición y Uso (--var, var()) | variables CSS]] y anidarse, lo que lo hace muy potente para sistemas de diseño:

```css
:root { --gap: 1rem; --cols: 3; }
.item {
  width: calc((100% - (var(--cols) - 1) * var(--gap)) / var(--cols));
}
```

## División por cero y tipos

> [!info] Reglas internas
> - El **divisor** de `/` debe ser un número **sin unidad** (`calc(100% / 3)`, no `calc(100% / 3px)`).
> - Una **división por cero** invalida la declaración.
> - `calc()` respeta la **precedencia** matemática (`*` y `/` antes que `+` y `-`); usa paréntesis para agrupar.

## calc() vs. clamp()/min()/max()

`calc()` calcula **un** valor; cuando necesitas **acotar** entre límites (un mínimo, un máximo, un rango fluido), las funciones [[07 min(), max(), clamp() | `min()`/`max()`/`clamp()`]] son más adecuadas y a menudo reemplazan combinaciones complejas de `calc()`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `calc()` para mezclar unidades fijas y fluidas (`100% - 2rem`).
> - **Siempre** espacios alrededor de `+` y `-`.
> - Combínalo con variables CSS para sistemas de espaciado/grid.
> - Para acotar valores entre límites, prefiere `clamp()`/`min()`/`max()`.

## Errores comunes

> [!warning] Trampas
> - **Faltar espacios** en `+`/`-`: invalida el cálculo.
> - **Dividir por una cantidad con unidad** o por cero.
> - **Abusar de `calc()` anidado** cuando `clamp()` sería más claro.

## Notas relacionadas

- [[07 min(), max(), clamp()]] — acotar valores, no solo calcular.
- [[01 Definición y Uso (--var, var())]] — variables dentro de `calc()`.
- [[05 Porcentajes]] — el tipo de valor que más se combina con `calc()`.
