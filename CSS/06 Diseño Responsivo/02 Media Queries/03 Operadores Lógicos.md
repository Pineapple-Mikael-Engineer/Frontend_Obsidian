---
title: Operadores Lógicos — Combinar condiciones (and, or, not)
aliases:
  - media query operators
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 3
---

# Operadores Lógicos

> [!definicion]
> Las media queries combinan varias condiciones con operadores lógicos: **`and`** (todas deben cumplirse), la **coma** (`,`, equivale a "o": basta una), y **`not`** (negar). Permiten condiciones precisas como "pantallas anchas en orientación horizontal".

```css
@media (min-width: 768px) and (orientation: landscape) { }
@media (max-width: 600px), (orientation: portrait) { }   /* o */
```

## Los operadores

| Operador | Significa | Ejemplo |
|----------|-----------|---------|
| `and` | **Todas** las condiciones | `(min-width: 768px) and (max-width: 1024px)` |
| `,` (coma) | **Alguna** (OR) | `(max-width: 600px), (orientation: portrait)` |
| `not` | **Negar** la query completa | `not all and (min-width: 768px)` |
| `only` | Solo navegadores que entienden media queries (legado) | `only screen and (...)` |

## and: rangos y combinaciones

`and` exige que **todas** las condiciones se cumplan, lo que crea rangos (entre dos anchos) o combinaciones (ancho + orientación):

```css
/* Solo tablets (entre 768 y 1024) */
@media (min-width: 768px) and (max-width: 1024px) { }

/* Pantallas anchas en horizontal */
@media (min-width: 900px) and (orientation: landscape) { }
```

## La coma es un OR

> [!info] La coma separa queries alternativas
> La **coma** funciona como un "o": los estilos se aplican si se cumple **al menos una** de las queries separadas por comas:
> ```css
> @media (max-width: 600px), (orientation: portrait) {
>   /* se aplica en pantallas estrechas O en orientación vertical */
> }
> ```
> Cada query separada por comas se evalúa de forma **independiente**; basta una verdadera. (No hay un operador `or` literal; la coma lo cumple.)

## not: negar con cuidado

> [!warning] not niega la query entera, no una feature
> `not` invierte **toda** la media query, no una condición concreta. Y requiere especificar el tipo de medio:
> ```css
> @media not all and (min-width: 768px) {
>   /* se aplica cuando NO es (pantalla ≥ 768px) → es decir, < 768px */
> }
> ```
> Es menos legible que usar `max-width` directamente, así que `not` se usa poco. Para "menos de 768px", `(max-width: 767px)` es más claro.

## La sintaxis de rangos lo simplifica

> [!tip] Los rangos modernos evitan and
> La sintaxis de rangos de Media Queries 4 reemplaza muchos `and`:
> ```css
> /* Antiguo: dos condiciones con and */
> @media (min-width: 768px) and (max-width: 1024px) { }
> /* Moderno: un rango */
> @media (768px <= width <= 1024px) { }
> ```
> Más legible y sin riesgo de off-by-one entre los límites.

## Buenas prácticas

> [!tip] Recomendaciones
> - `and` para rangos y combinaciones (ancho + orientación/preferencia).
> - La coma para condiciones alternativas (OR).
> - Evita `not`: suele ser más claro usar `max-width` o un rango.
> - Prefiere la sintaxis de rangos (`a <= width <= b`) a los `and` de min/max.

## Errores comunes

> [!warning] Trampas
> - **Esperar un operador `or`**: usa la coma.
> - **`not` mal entendido**: niega toda la query, no una feature.
> - **Rangos solapados** con min/max (off-by-one): los rangos `<=`/`>=` lo evitan.

## Notas relacionadas

- [[01 Sintaxis @media]] — la estructura base.
- [[04 Dimensión (width, height, aspect-ratio)]] — las features que se combinan.
- [[07 Breakpoints Comunes]] — los rangos típicos.
