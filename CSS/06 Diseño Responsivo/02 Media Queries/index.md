---
title: Media Queries — Estilos condicionales según el dispositivo
aliases:
  - media queries
  - "@media"
tags:
  - css
  - api/concepto
  - responsive
draft: false
---

# Media Queries

> [!definicion]
> Una **media query** aplica estilos solo cuando se cumple una **condición** sobre el dispositivo: un ancho de pantalla, una orientación, una preferencia del usuario, un tipo de puntero. Se escriben con la regla `@media`. Son la herramienta clásica del diseño responsivo "por breakpoints".

```css
@media (min-width: 768px) {
  .menu { display: flex; }   /* solo en pantallas ≥ 768px */
}
```

## Mapa de la subsección

- [[01 Sintaxis @media]] — la estructura de una media query.
- [[02 Tipos de Medio]] — `screen`, `print`, `all`.
- [[03 Operadores Lógicos]] — combinar condiciones (`and`, `or`, `not`).
- [[04 Dimensión (width, height, aspect-ratio)]] — las features de tamaño.
- [[05 Interacción (hover, pointer)]] — detectar ratón vs. táctil.
- [[06 Preferencias (prefers-color-scheme, prefers-reduced-motion)]] — preferencias del usuario.
- [[07 Breakpoints Comunes]] — los puntos de ruptura habituales.

## Lo que se puede consultar

| Categoría | Features | Nota |
|-----------|----------|------|
| Dimensión | `width`, `height`, `aspect-ratio` | [[04 Dimensión (width, height, aspect-ratio)]] |
| Orientación | `orientation: portrait/landscape` | [[04 Dimensión (width, height, aspect-ratio)]] |
| Interacción | `hover`, `pointer` | [[05 Interacción (hover, pointer)]] |
| Preferencias | `prefers-color-scheme`, `prefers-reduced-motion` | [[06 Preferencias (prefers-color-scheme, prefers-reduced-motion)]] |
| Resolución | `resolution` (pantallas retina) | — |

## No solo es para el tamaño

> [!tip] Las media queries van más allá del ancho
> Aunque se asocian con "responsive por ancho", las media queries consultan mucho más: si el usuario prefiere **modo oscuro**, si tiene **animaciones reducidas** activadas, si usa **ratón o táctil**, la **orientación** del dispositivo. Estas consultas de **preferencias** y **capacidades** son tan importantes como las de ancho para una experiencia adaptada de verdad. Detalle en [[06 Preferencias (prefers-color-scheme, prefers-reduced-motion)]] y [[05 Interacción (hover, pointer)]].

## Menos media queries con CSS moderno

> [!info] El CSS fluido reduce los breakpoints
> Muchos layouts que antes necesitaban varias media queries hoy se resuelven sin ninguna, con [[07 min(), max(), clamp() | `clamp()`]] (tipografía/espaciado fluidos), [[12 Patrones (auto-fit, auto-fill) | `auto-fit`]] (rejillas) y [[07 Container Queries/index | container queries]]. Las media queries siguen siendo necesarias para **cambios estructurales** grandes (un menú que pasa a hamburguesa), pero el CSS fluido recorta su número.

## Notas relacionadas

- [[01 Sintaxis @media]] — el punto de partida.
- [[07 Breakpoints Comunes]] — dónde poner los cortes.
- [[07 Container Queries/index]] — la alternativa basada en el contenedor.
