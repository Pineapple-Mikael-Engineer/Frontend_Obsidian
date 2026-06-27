---
title: Múltiples Animaciones — Varias a la vez en un elemento
aliases:
  - multiple animations
tags:
  - css
  - api/concepto
  - animacion
draft: false
order: 8
---

# Múltiples Animaciones

> [!definicion]
> Un elemento puede tener **varias animaciones** simultáneas, separadas por comas en la propiedad `animation`. Cada una con su nombre, duración, curva y demás. Permite componer efectos complejos (un elemento que entra **y** pulsa, o que se mueve en dos ejes con ritmos distintos).

```css
.elemento {
  animation:
    entrar 0.5s ease-out,
    flotar 3s ease-in-out infinite;
}
```

## La sintaxis: comas

Cada animación es un conjunto de valores separado del siguiente por una **coma**:

```css
animation:
  <name1> <dur1> <timing1> <delay1> <count1> ...,
  <name2> <dur2> <timing2> ...;
```

```css
animation:
  aparecer 0.4s ease-out forwards,
  pulso 2s ease-in-out 0.4s infinite;
```

## Componer efectos independientes

> [!tip] Una animación por aspecto
> El valor de las animaciones múltiples está en **separar responsabilidades**: una animación para la entrada, otra para un efecto continuo, otra para el movimiento. Cada una con su ritmo:
> ```css
> @keyframes entrar { from { opacity: 0; } to { opacity: 1; } }
> @keyframes flotar { to { transform: translateY(-8px); } }
> .icono {
>   animation:
>     entrar 0.5s ease-out forwards,
>     flotar 2s ease-in-out infinite alternate;
> }
> /* aparece una vez Y flota en bucle */
> ```

## El conflicto de transform

> [!warning] Dos animaciones no pueden tocar la misma propiedad
> El gran límite: si **dos animaciones** intentan animar la **misma propiedad** (p. ej. ambas `transform`), la **última** gana y la otra se ignora. No se combinan. Por eso animar el movimiento horizontal y vertical con dos `@keyframes` de `transform` separadas **no funciona**:
> ```css
> /* ❌ las dos tocan transform: solo una se aplica */
> animation: moverX 2s infinite, moverY 3s infinite;
> ```
> Soluciones:
> - Combinar ambos movimientos en **una sola** `@keyframes` con `transform: translate(x, y)`.
> - Usar las **propiedades individuales** modernas (`translate`, `rotate`, `scale`), que sí se pueden animar por separado sin pisarse.
> ```css
> /* ✅ propiedades individuales: cada animación toca una */
> .x { animation: girar 2s infinite, latir 1s infinite; }
> @keyframes girar { to { rotate: 360deg; } }
> @keyframes latir { 50% { scale: 1.1; } }
> ```

## Listas paralelas en las sub-propiedades

Si usas las propiedades sueltas (no el shorthand), cada una acepta una lista paralela, una entrada por animación:

```css
animation-name: entrar, flotar;
animation-duration: 0.5s, 3s;
animation-iteration-count: 1, infinite;
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Separa efectos independientes en animaciones distintas (entrada + bucle continuo).
> - Para combinar movimientos que tocan `transform`, usa **una** `@keyframes` o las propiedades individuales (`translate`/`rotate`/`scale`).
> - Mantén el orden coherente si usas listas paralelas en las sub-propiedades.
> - No abuses: demasiadas animaciones simultáneas complican el rendimiento y el mantenimiento.

## Errores comunes

> [!warning] Trampas
> - **Dos animaciones sobre `transform`**: solo una se aplica.
> - **Listas paralelas desalineadas** en las sub-propiedades.
> - **Sobrecargar** un elemento con muchas animaciones (rendimiento).

## Notas relacionadas

- [[01 Transformaciones 2D (translate, rotate, scale, skew)]] — las propiedades individuales que evitan el conflicto.
- [[03 animation-timing-function y delay]] — desfasar animaciones múltiples.
- [[03 Animar transform y opacity]] — el rendimiento de varias animaciones.
