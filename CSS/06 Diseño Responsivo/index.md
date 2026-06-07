---
title: Diseño Responsivo — Una web que se adapta a cualquier pantalla
aliases:
  - responsive design
tags:
  - css
  - api/concepto
  - responsive
draft: false
---

# Diseño Responsivo

> [!definicion]
> El **diseño responsivo** hace que una misma web se vea y funcione bien en **cualquier pantalla**: móvil, tablet, escritorio. En vez de versiones separadas, un único CSS se **adapta** al tamaño y las capacidades del dispositivo, mediante el [[01 Viewport/index | viewport]], las [[02 Media Queries/index | media queries]], unidades fluidas y, ahora, las [[07 Container Queries/index | container queries]].

```css
.contenedor { width: 100%; max-width: 65rem; }   /* fluido con tope */

@media (min-width: 768px) {
  .layout { grid-template-columns: 250px 1fr; }   /* dos columnas en pantallas anchas */
}
```

## Mapa de la sección

- [[01 Viewport/index]] — el `<meta viewport>`, prerrequisito de todo lo responsivo.
- [[02 Media Queries/index]] — adaptar el CSS al tamaño y capacidades del dispositivo.
- [[03 Enfoques de Diseño/index]] — mobile-first, desktop-first, diseño fluido.
- [[04 Imágenes Responsivas]] — servir imágenes adecuadas a cada pantalla.
- [[05 Tipografía Responsiva/index]] — texto que escala con el viewport.
- [[06 Layouts Responsivos/index]] — patrones (rejillas, menús, tablas).
- [[07 Container Queries/index]] — adaptar al tamaño del **contenedor**, no del viewport.

## Las dos filosofías

> [!info] Adaptar por puntos vs. fluir
> Hay dos formas (complementarias) de hacer responsive:
> 1. **Por breakpoints** (media queries): el layout cambia en saltos a ciertos anchos (móvil → tablet → escritorio).
> 2. **Fluido** (unidades relativas, `clamp()`, `auto-fit`): el diseño se adapta **continuamente** sin saltos.
>
> El enfoque moderno combina ambos: tanto fluidez (que reduce el número de breakpoints necesarios) como media queries para los cambios estructurales grandes. Muchos layouts que antes necesitaban varias media queries hoy se resuelven con [[07 min(), max(), clamp() | `clamp()`]] y [[12 Patrones (auto-fit, auto-fill) | `auto-fit`]] sin ninguna.

## El cambio de paradigma: container queries

> [!tip] Del viewport al contenedor
> Durante años, "responsive" significaba reaccionar al **viewport** (el tamaño de la ventana). El problema: un componente no sabe en qué contexto está (¿ocupa toda la pantalla o una columna estrecha?). Las [[07 Container Queries/index | container queries]] resuelven esto: un componente se adapta al tamaño de **su contenedor**, no de la ventana. Es el avance más importante del responsive reciente y permite componentes verdaderamente reutilizables.

## El móvil primero

La práctica dominante es **mobile-first**: diseñar para la pantalla pequeña por defecto y **añadir** complejidad para pantallas mayores con `min-width`. Detalle en [[01 Mobile First]].

## Notas relacionadas

- [[02 Media Queries/index]] — la herramienta central.
- [[07 Container Queries/index]] — el futuro del responsive.
- [[02 Viewport (meta viewport)]] — el meta tag desde HTML.
