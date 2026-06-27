---
title: Layouts Responsivos — Patrones que se adaptan
aliases:
  - responsive layouts
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 5
---

# Layouts Responsivos

> [!definicion]
> Un puñado de **patrones** resuelven la mayoría de las necesidades de layout responsivo: rejillas que se reorganizan, componentes que se reflujan, menús que se colapsan en hamburguesa y tablas que se adaptan a móvil. Esta subsección reúne las recetas más útiles.

## Mapa de la subsección

- [[01 Flex-wrap para Componentes]] — componentes que se reflujan con Flexbox.
- [[02 Grid con auto-fit]] — la rejilla responsiva por excelencia.
- [[03 Menús Hamburguesa]] — el menú que se colapsa en móvil.
- [[04 Tablas Responsivas]] — adaptar tablas a pantallas estrechas.

## Lo gradual vs. lo estructural

> [!info] Dos tipos de adaptación
> Los patrones se dividen según cómo se adaptan:
> - **Gradual** (sin media queries): rejillas con [[02 Grid con auto-fit | `auto-fit`]] y componentes con [[01 Flex-wrap para Componentes | `flex-wrap`]] que se reorganizan solos según el espacio.
> - **Estructural** (con media queries o JS): cambios que **no** se pueden interpolar, como un [[03 Menús Hamburguesa | menú que se vuelve hamburguesa]] o una [[04 Tablas Responsivas | tabla que cambia de formato]].
>
> El diseño moderno usa lo gradual siempre que puede (menos código, más robusto) y lo estructural solo cuando es imprescindible.

## La caja de herramientas

| Patrón | Herramienta principal |
|--------|----------------------|
| Galería que reorganiza columnas | `grid` + `auto-fit` + `minmax()` |
| Componentes que se reflujan | `flex-wrap` |
| Menú que colapsa | media query + JS (toggle) |
| Tabla que se adapta | scroll, o reorganización con CSS |

## Notas relacionadas

- [[02 Grid con auto-fit]] — la receta más usada.
- [[03 Flexbox/index]] · [[04 CSS Grid/index]] — los sistemas de layout subyacentes.
- [[03 Diseño Fluido]] — la filosofía de la adaptación gradual.
