---
title: Enfoques de Diseño — Mobile-first, desktop-first, fluido
aliases:
  - design approaches
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 3
---

# Enfoques de Diseño

> [!definicion]
> Hay distintas **estrategias** para construir un diseño responsivo: empezar por el móvil y crecer ([[01 Mobile First | mobile-first]]), empezar por el escritorio y reducir ([[02 Desktop First | desktop-first]]), o evitar saltos con un [[03 Diseño Fluido | diseño fluido]]. La elección afecta a cómo se escriben las media queries.

## Mapa de la subsección

- [[01 Mobile First]] — base móvil + `min-width` para crecer (recomendado).
- [[02 Desktop First]] — base escritorio + `max-width` para reducir.
- [[03 Diseño Fluido]] — adaptación continua sin breakpoints.

## La diferencia esencial

> [!info] min-width vs. max-width
> Los dos enfoques con media queries se distinguen por la dirección:
> - **Mobile-first**: el CSS base es para móvil; las media queries de **`min-width`** **añaden** estilos al crecer la pantalla.
> - **Desktop-first**: el CSS base es para escritorio; las media queries de **`max-width`** **adaptan** al reducir.
>
> Mobile-first es el enfoque **recomendado**: empieza por lo simple (móvil) y añade complejidad, lo que produce CSS más limpio y mejor rendimiento en móvil (donde menos recursos hay). Detalle en [[01 Mobile First]].

## Lo fluido por encima de los breakpoints

> [!tip] Combinar fluidez y breakpoints
> El enfoque más moderno no es elegir uno u otro, sino **combinar**: hacer el diseño lo más **fluido** posible (con `clamp()`, `auto-fit`, `%`) para que se adapte solo, y usar **media queries** únicamente para los cambios estructurales que la fluidez no puede hacer (un menú que se vuelve hamburguesa). Esto reduce los breakpoints al mínimo y hace el diseño robusto ante cualquier tamaño. Detalle en [[03 Diseño Fluido]].

## Notas relacionadas

- [[01 Mobile First]] — el enfoque recomendado.
- [[03 Diseño Fluido]] — adaptación sin saltos.
- [[07 Breakpoints Comunes]] — dónde poner los cortes.
