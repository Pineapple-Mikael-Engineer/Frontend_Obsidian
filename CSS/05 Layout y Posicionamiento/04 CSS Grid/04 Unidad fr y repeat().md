---
title: Unidad fr y repeat() — Repartir espacio y repetir pistas
aliases:
  - fr unit
  - repeat
tags:
  - css
  - api/concepto
  - layout
draft: false
order: 4
---

# Unidad fr y repeat()

> [!definicion]
> La unidad **`fr`** (fracción) reparte el **espacio sobrante** de la rejilla entre las pistas, de forma proporcional. La función **`repeat()`** abrevia la definición de pistas repetidas. Juntas son la base de casi toda rejilla Grid.

```css
grid-template-columns: repeat(3, 1fr);   /* 3 columnas iguales */
grid-template-columns: 1fr 2fr;          /* la segunda, doble de ancha */
```

## La unidad fr

> [!info] fr reparte lo que sobra, descontando el gap
> `1fr` significa "una fracción del espacio **disponible**". Con `1fr 1fr 1fr`, el espacio se reparte en tres partes iguales. Con `1fr 2fr`, en tres partes, dando una a la primera y dos a la segunda:
> ```css
> grid-template-columns: 1fr 2fr 1fr;   /* 25% / 50% / 25% del sobrante */
> ```
> La gran ventaja sobre `%`: **`fr` descuenta automáticamente el `gap`**. Con `repeat(3, 1fr)` y `gap: 1rem`, las columnas se reparten el espacio que queda tras restar los gaps, sin desbordar. Con `%`, el gap se sumaría y desbordaría.

## Mezclar fr con tamaños fijos

`fr` convive con pistas fijas: las fijas toman su tamaño y `fr` reparte el **resto**:

```css
/* Barra lateral de 250px + contenido que ocupa el resto */
grid-template-columns: 250px 1fr;

/* Dos barras fijas y un centro flexible */
grid-template-columns: 200px 1fr 200px;
```

## repeat(): no repetir valores a mano

> [!tip] repeat para pistas uniformes
> `repeat(n, patrón)` evita escribir el mismo valor muchas veces:
> ```css
> grid-template-columns: repeat(4, 1fr);        /* = 1fr 1fr 1fr 1fr */
> grid-template-columns: repeat(3, 100px 1fr);  /* repite el par 3 veces (6 columnas) */
> ```

## repeat con auto-fit / auto-fill

> [!tip] La rejilla responsiva automática
> El uso más potente de `repeat()` combina `auto-fit`/`auto-fill` con `minmax()` para crear rejillas que **ajustan el número de columnas** al ancho disponible, sin media queries:
> ```css
> grid-template-columns: repeat(auto-fit, minmax(15rem, 1fr));
> ```
> Cada columna mide al menos 15rem; el navegador mete tantas como quepan y reparte el sobrante. Es la receta de galería responsiva por excelencia. Detalle en [[12 Patrones (auto-fit, auto-fill)]].

## fr vs. porcentajes

| | `fr` | `%` |
|--|------|-----|
| Descuenta el `gap` | **Sí** | No |
| Reparte el sobrante | Sí | Es del contenedor total |
| Riesgo de desbordar con gap | No | Sí |

Por eso en Grid se prefiere `fr` a `%` para columnas flexibles.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `fr` para columnas flexibles: reparte el sobrante y descuenta el `gap`.
> - `repeat(n, 1fr)` para n columnas iguales.
> - `repeat(auto-fit, minmax(<min>, 1fr))` para rejillas responsivas automáticas.
> - Combina pistas fijas (`px`/`rem`) con `1fr` para layouts barra+contenido.

## Errores comunes

> [!warning] Trampas
> - **Usar `%`** en vez de `fr` y desbordar por culpa del `gap`.
> - **Repetir valores a mano** en vez de `repeat()`.
> - **Olvidar `minmax()`** con `auto-fit` (la rejilla responsiva lo necesita).

## Notas relacionadas

- [[03 grid-template-columns y rows]] — donde se usan `fr` y `repeat()`.
- [[05 minmax() y fit-content()]] — el `minmax()` de las rejillas responsivas.
- [[12 Patrones (auto-fit, auto-fill)]] — `auto-fit`/`auto-fill`.
