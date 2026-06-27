---
title: CSS — Documentación de referencia
aliases:
  - CSS
  - Cascading Style Sheets
tags:
  - css
  - api/concepto
draft: false
order: 1
---

# CSS

> [!definicion]
> CSS (Cascading Style Sheets) es el lenguaje de hojas de estilo que controla la presentación visual de documentos HTML. Sus dos mecanismos centrales son la **cascada** — el algoritmo que resuelve conflictos cuando múltiples reglas apuntan al mismo elemento — y la **herencia** — la propagación automática de valores desde un elemento padre a sus descendientes.

La unidad mínima de CSS es la regla: un selector que identifica qué elementos se afectan, seguido de declaraciones `propiedad: valor`. La cascada pondera origen, especificidad y orden para decidir qué declaración gana:

```css
/* Especificidad: 0-1-1 → un selector de clase + un elemento */
p.intro {
  color: #1a1a2e;       /* declaración ganadora si no hay regla más específica */
  font-size: 1rem;
}

/* La herencia propaga 'color' y 'font-size' a los hijos de <p> */
```

## Contenido de la sección

| # | Nombre | Cubre |
|---|---|---|
| 01 | Fundamentos del Estilo | Sintaxis de reglas, selectores (tipo, clase, ID, atributo, combinadores), especificidad, cascada y herencia |
| 02 | Modelo de Caja | `box-sizing`, márgenes, bordes, relleno, colapso de márgenes y el flujo normal del documento |
| 03 | Tipografía Avanzada | `@font-face`, `font-display`, propiedades tipográficas (tracking, leading, optical sizing), `line-height` y escalas de tipo |
| 04 | Fondos y Efectos Visuales | `background-*`, gradientes, filtros (`filter`, `backdrop-filter`), `mix-blend-mode`, sombras y `clip-path` |
| 05 | Layout y Posicionamiento | Flexbox, Grid, `position` (static/relative/absolute/fixed/sticky) y contextos de apilamiento (`z-index`) |
| 06 | Diseño Responsivo | Media queries, container queries, unidades relativas (`vw`, `vh`, `dvh`, `cqi`), imágenes fluidas y estrategia mobile-first |
| 07 | Animaciones y Transiciones | `transition`, `@keyframes`, `animation-*`, `will-change`, motion path y consideraciones de rendimiento |
| 08 | Pseudoclases y Pseudoelementos | `:is()`, `:where()`, `:has()`, `:not()`, pseudoelementos (`::before`, `::after`, `::marker`, `::selection`) |
| 09 | Arquitectura y Metodologías | BEM, SMACSS, ITCSS, Utility-first (Tailwind), capas (`@layer`) y estrategias de naming para proyectos grandes |
| 10 | Variables CSS | Custom properties (`--token`), `var()`, alcance con `:root` vs. componente, temas dinámicos y `@property` |

## Arco de aprendizaje

Las diez secciones forman una progresión deliberada de lo fundamental a lo sistémico. Las secciones 01 y 02 establecen el vocabulario base: cómo CSS resuelve conflictos (cascada, especificidad) y cómo cada elemento ocupa espacio (modelo de caja); sin estos dos pilares todo lo demás es sintaxis huérfana. Las secciones 03 y 04 amplían el control visual hacia tipografía de calidad y efectos de fondo, áreas donde una línea de CSS bien elegida reemplaza varios assets de imagen. El bloque 05-06 cubre el núcleo del trabajo moderno en UI: Flexbox y Grid para estructura interna, y media/container queries para adaptar esa estructura a cualquier viewport. Las secciones 07 y 08 añaden dinamismo — transiciones, animaciones y la nueva generación de selectores funcionales (`:has()`, `:is()`) — sin necesidad de JavaScript. Finalmente, 09 y 10 elevan la perspectiva al nivel de sistema: metodologías de arquitectura para proyectos escalables y custom properties como capa de tokens que conecta diseño con código.

## Notas relacionadas

- [[01 Fundamentos del Estilo/index|Fundamentos del Estilo]]
- [[02 Modelo de Caja/index|Modelo de Caja]]
- [[03 Tipografía Avanzada/index|Tipografía Avanzada]]
- [[04 Fondos y Efectos Visuales/index|Fondos y Efectos Visuales]]
- [[05 Layout y Posicionamiento/index|Layout y Posicionamiento]]
- [[06 Diseño Responsivo/index|Diseño Responsivo]]
- [[07 Animaciones y Transiciones/index|Animaciones y Transiciones]]
- [[08 Pseudoclases y Pseudoelementos|Pseudoclases y Pseudoelementos]]
- [[09 Arquitectura y Metodologías/index|Arquitectura y Metodologías]]
- [[10 Variables CSS/index|Variables CSS]]
