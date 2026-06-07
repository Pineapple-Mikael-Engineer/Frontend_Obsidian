---
title: Mobile First — Diseñar desde la pantalla pequeña
aliases:
  - mobile first
tags:
  - css
  - api/concepto
  - responsive
draft: false
---

# Mobile First

> [!definicion]
> **Mobile-first** es la estrategia de diseñar primero para la **pantalla pequeña** (móvil) como caso base, y **añadir** complejidad para pantallas mayores con media queries de `min-width`. Es el enfoque recomendado: produce CSS más limpio y mejor rendimiento en móvil.

```css
/* Base: móvil (una columna) */
.layout { display: grid; gap: 1rem; }

/* Se añade en tablet+ */
@media (min-width: 768px) {
  .layout { grid-template-columns: 250px 1fr; }
}
```

## La estructura mobile-first

> [!info] Base simple + min-width para crecer
> El patrón:
> 1. El **CSS base** (fuera de media queries) describe la versión móvil: una columna, menú apilado, todo simple.
> 2. Las media queries de **`min-width`** **añaden** estilos a medida que hay más espacio: columnas, barra lateral, menú horizontal.
>
> Se construye "de menos a más": empiezas con lo esencial y enriqueces. Las media queries solo **añaden**, no deshacen.

## Por qué es mejor que desktop-first

> [!tip] Las ventajas del mobile-first
> - **CSS más limpio**: añadir estilos (`min-width`) es más natural que deshacerlos (`max-width`). Menos sobrescrituras.
> - **Rendimiento en móvil**: el dispositivo con menos recursos recibe el CSS más simple por defecto; las mejoras de escritorio van en media queries que el móvil ni procesa.
> - **Prioriza el contenido**: diseñar para la pantalla pequeña obliga a centrarse en lo esencial primero.
> - **Refleja el uso real**: la mayoría del tráfico web es móvil.

## Ejemplo completo

```css
/* === BASE (móvil) === */
.nav { display: flex; flex-direction: column; }
.galeria { display: grid; gap: 1rem; }          /* una columna */
h1 { font-size: 1.75rem; }

/* === TABLET (768px+) === */
@media (min-width: 768px) {
  .nav { flex-direction: row; }                  /* menú horizontal */
  .galeria { grid-template-columns: repeat(2, 1fr); }
  h1 { font-size: 2.25rem; }
}

/* === ESCRITORIO (1024px+) === */
@media (min-width: 1024px) {
  .galeria { grid-template-columns: repeat(3, 1fr); }
  h1 { font-size: 3rem; }
}
```

## El orden ascendente

> [!warning] Las media queries van de menor a mayor
> En mobile-first, las media queries de `min-width` deben ordenarse de **menor a mayor** ancho, para que la cascada funcione: una pantalla de 1200px cumple `768px` **y** `1024px`, y la última en el código gana. Si las pones en orden inverso, los estilos de 768 sobrescribirían a los de 1024:
> ```css
> @media (min-width: 768px)  { … }   /* primero */
> @media (min-width: 1024px) { … }   /* después: gana en pantallas grandes */
> ```

## Combinar con fluidez

Mobile-first se combina con el [[03 Diseño Fluido | diseño fluido]]: la base móvil ya usa `clamp()` y `auto-fit`, así que muchos ajustes ocurren sin media queries, y estas quedan para los cambios estructurales grandes.

## Buenas prácticas

> [!tip] Recomendaciones
> - Diseña la versión móvil como base (fuera de media queries).
> - Usa `min-width` para **añadir** complejidad en pantallas mayores.
> - Ordena las media queries de menor a mayor ancho.
> - Combínalo con CSS fluido para reducir el número de breakpoints.

## Errores comunes

> [!warning] Trampas
> - **Mezclar `min-width` y `max-width`** sin orden claro: cascada confusa.
> - **Orden descendente** de los `min-width`: los breakpoints pequeños pisan a los grandes.
> - **Diseñar para escritorio primero** y luego "arreglar" el móvil: más sobrescrituras y peor rendimiento.

## Notas relacionadas

- [[02 Desktop First]] — el enfoque opuesto.
- [[03 Diseño Fluido]] — la fluidez que complementa al mobile-first.
- [[01 Sintaxis @media]] — la sintaxis de `min-width`.
