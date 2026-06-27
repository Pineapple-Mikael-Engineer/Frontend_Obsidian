---
title: "Sintaxis @media — La estructura de una media query"
aliases:
  - media query syntax
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 1
---

# Sintaxis @media

> [!definicion]
> Una media query empieza con `@media`, seguido de una **condición** entre paréntesis, y un bloque de reglas que se aplican **solo si** la condición se cumple. Es una "envoltura condicional" alrededor de CSS normal.

```css
@media (min-width: 768px) {
  .columna { width: 50%; }
  .menu { display: flex; }
}
```

## La estructura

```css
@media <tipo> and (<feature>: <valor>) {
  /* reglas que se aplican si se cumple */
}
```

| Parte | Ejemplo | Opcional |
|-------|---------|----------|
| `@media` | — | No |
| Tipo de medio | `screen`, `print` | Sí (por defecto `all`) |
| Feature | `(min-width: 768px)` | — |
| Bloque | `{ ... }` | No |

## Las features de ancho: min-width y max-width

> [!info] min-width vs. max-width
> Las dos features más usadas:
> - **`min-width: 768px`**: se aplica cuando el viewport es **768px o más** (pantallas grandes). Base del enfoque [[01 Mobile First | mobile-first]].
> - **`max-width: 767px`**: se aplica cuando el viewport es **767px o menos** (pantallas pequeñas). Base de desktop-first.
>
> ```css
> @media (min-width: 768px) { /* tablet y escritorio */ }
> @media (max-width: 767px) { /* móvil */ }
> ```

## La sintaxis de rangos moderna

> [!tip] Comparadores < > en vez de min/max
> CSS Media Queries 4 permite **comparadores** directos, más legibles que `min-`/`max-`:
> ```css
> /* Antiguo */
> @media (min-width: 768px) and (max-width: 1024px) { }
> /* Moderno (rango) */
> @media (768px <= width <= 1024px) { }
> @media (width >= 768px) { }
> ```
> El rango con `<=`/`>=` evita el clásico problema del "off-by-one" (¿767 o 768?). El soporte ya es amplio.

## Las media queries cascadean

> [!info] Se aplican como reglas normales
> El CSS dentro de una `@media` participa en la **cascada** como cualquier regla: misma especificidad por el selector, y el **orden** importa. Por eso, en mobile-first, las media queries de `min-width` van **después** del CSS base para sobrescribirlo en pantallas grandes:
> ```css
> .caja { width: 100%; }                      /* base (móvil) */
> @media (min-width: 768px) {
>   .caja { width: 50%; }                      /* sobrescribe en grande */
> }
> ```

## Anidar media queries (CSS moderno)

Con la anidación de CSS, las media queries se pueden poner **dentro** de una regla, junto al selector que modifican:

```css
.caja {
  width: 100%;
  @media (min-width: 768px) { width: 50%; }   /* anidada */
}
```

Mantiene los estilos de un componente juntos. El soporte de anidación es moderno.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `min-width` para mobile-first (lo más recomendado).
> - Considera la sintaxis de rangos (`width >= 768px`) por legibilidad.
> - Coloca las media queries **después** del CSS base para que lo sobrescriban.
> - Con CSS moderno, anida las media queries dentro del componente que afectan.

## Errores comunes

> [!warning] Trampas
> - **Orden incorrecto**: una media query antes del CSS base no lo sobrescribe.
> - **Solape de rangos** (`max-width: 768px` y `min-width: 768px` ambos en 768): off-by-one.
> - **Olvidar el viewport meta**: las media queries de ancho no funcionan en móvil.

## Notas relacionadas

- [[04 Dimensión (width, height, aspect-ratio)]] — las features de tamaño.
- [[03 Operadores Lógicos]] — combinar condiciones.
- [[01 Mobile First]] — el enfoque con `min-width`.
