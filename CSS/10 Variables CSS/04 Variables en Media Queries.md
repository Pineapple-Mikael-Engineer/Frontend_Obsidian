---
title: Variables CSS en Media Queries
aliases:
  - variables en media queries
  - custom properties responsive
tags:
  - css
  - api/concepto
  - variables
draft: false
---

# Variables en Media Queries

> [!definicion]
> Las variables CSS se pueden redefinir dentro de `@media`, lo que permite cambiar valores en distintos breakpoints desde un único punto de control, sin necesidad de repetir selectores. En lugar de duplicar las declaraciones de un componente en cada breakpoint, se redefine solo la variable que cambia.

```css
:root {
  --columns: 1;
  --font-size-heading: 1.5rem;
  --space-section: 2rem;
}

@media (min-width: 768px) {
  :root {
    --columns: 2;
    --font-size-heading: 2rem;
    --space-section: 4rem;
  }
}

@media (min-width: 1024px) {
  :root {
    --columns: 3;
    --font-size-heading: 2.5rem;
    --space-section: 6rem;
  }
}
```

## El patrón: cambiar variables, no selectores

Sin variables CSS, el diseño responsivo requiere repetir los selectores en cada breakpoint:

```css
/* SIN variables — repetición de selectores */
.grid { grid-template-columns: 1fr; gap: 1rem; }

@media (min-width: 768px) {
  .grid { grid-template-columns: 1fr 1fr; gap: 1.5rem; }
}

@media (min-width: 1024px) {
  .grid { grid-template-columns: repeat(3, 1fr); gap: 2rem; }
}
```

Con variables, los selectores se escriben una sola vez. Los breakpoints solo tocan las variables:

```css
/* CON variables — selector una sola vez */
:root {
  --grid-cols: 1;
  --grid-gap: 1rem;
}

@media (min-width: 768px) { :root { --grid-cols: 2; --grid-gap: 1.5rem; } }
@media (min-width: 1024px) { :root { --grid-cols: 3; --grid-gap: 2rem; } }

.grid {
  display: grid;
  grid-template-columns: repeat(var(--grid-cols), 1fr);
  gap: var(--grid-gap);
}
```

## Variables de tipografía responsiva

En lugar de usar `clamp()` para todo, las variables en media queries permiten escalar tipografía con control en pasos discretos:

```css
:root {
  --text-xs: 0.75rem;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.125rem;
  --text-xl: 1.25rem;
  --text-2xl: 1.5rem;
  --text-3xl: 1.875rem;
  --text-4xl: 2.25rem;
}

@media (min-width: 1024px) {
  :root {
    --text-3xl: 2.25rem;
    --text-4xl: 3rem;
  }
}

.hero__titulo { font-size: var(--text-4xl); }
.seccion__titulo { font-size: var(--text-3xl); }
```

## Variables de espaciado responsivo

El espaciado entre secciones es habitualmente mayor en pantallas más grandes. Con variables, se controla desde un solo lugar:

```css
:root {
  --space-section: 3rem;
  --space-componente: 1.5rem;
  --contenedor-max: 72rem;
  --contenedor-padding: 1rem;
}

@media (min-width: 768px) {
  :root {
    --space-section: 5rem;
    --space-componente: 2rem;
    --contenedor-padding: 2rem;
  }
}

@media (min-width: 1280px) {
  :root {
    --space-section: 8rem;
    --contenedor-padding: 3rem;
  }
}

.seccion { padding-block: var(--space-section); }
.contenedor { max-width: var(--contenedor-max); padding-inline: var(--contenedor-padding); }
```

## Combinar con @container

Container queries permiten redefinir variables según el **tamaño del contenedor**, no de la ventana:

```css
.card-container {
  container-type: inline-size;
  --card-layout: column;
  --card-img-width: 100%;
}

@container (min-width: 500px) {
  .card-container {
    --card-layout: row;
    --card-img-width: 40%;
  }
}

.card {
  display: flex;
  flex-direction: var(--card-layout);
}

.card__imagen { width: var(--card-img-width); }
```

## Recetas comunes

### Sistema de espaciado fluido con variables en breakpoints

```css
:root {
  /* Espaciado en móvil */
  --sp-xs: 0.25rem;
  --sp-sm: 0.5rem;
  --sp-md: 1rem;
  --sp-lg: 1.5rem;
  --sp-xl: 2rem;
  --sp-2xl: 3rem;
}

@media (min-width: 768px) {
  :root {
    --sp-lg: 2rem;
    --sp-xl: 3rem;
    --sp-2xl: 5rem;
  }
}
```

### Layout principal adaptable

```css
:root {
  --sidebar-width: 0;
  --main-max: 100%;
  --layout-cols: "main";
}

@media (min-width: 1024px) {
  :root {
    --sidebar-width: 20rem;
    --main-max: 72rem;
    --layout-cols: "sidebar main";
  }
}

.layout {
  display: grid;
  grid-template-areas: var(--layout-cols);
  grid-template-columns: var(--sidebar-width) 1fr;
  max-width: var(--main-max);
}
```

## Por qué no se pueden usar variables en la condición del @media

Una limitación importante: las variables CSS **no** pueden usarse en la condición de `@media`. Solo funcionan dentro de los bloques de declaraciones, no en los "media feature queries".

```css
:root { --breakpoint-md: 768px; }

/* INCORRECTO — no funciona */
@media (min-width: var(--breakpoint-md)) { }

/* CORRECTO — las variables van dentro del bloque */
@media (min-width: 768px) {
  :root { --columnas: 2; }
}
```

Para breakpoints reutilizables en las condiciones, Sass variables o CSS Media Query Range Syntax son las alternativas actuales. La propuesta `@custom-media` (de CSS Values Level 5) permitiría esto en el futuro, pero aún no tiene soporte estable.

## Cómo funciona por dentro

Cuando cambia el breakpoint activo, el navegador recalcula los valores de las custom properties para los elementos afectados por el `@media`. Esto desencadena la invalidación de todos los estilos que usan esas variables (mediante `var()`). El proceso es eficiente porque solo los nodos que realmente heredan o usan las variables se recomputan.

## Buenas prácticas

> [!tip] Recomendaciones
> - Redefine variables en `:root` dentro de `@media` en vez de repetir selectores de componentes en cada breakpoint.
> - Agrupa todos los cambios de un breakpoint en un solo bloque `@media { :root { ... } }`.
> - Usa nombres semánticos para las variables de layout: `--grid-cols`, `--sidebar-width`, no `--columnas-numericas`.
> - Para tipografía, considera combinar `clamp()` con variables para lo que cambia gradualmente, y breakpoints para cambios discretos.

## Errores comunes

> [!warning] Trampas
> - **Usar variables en la condición del @media**: `@media (min-width: var(--bp))` no funciona.
> - **Redefinir variables en el componente dentro del @media** en vez de en `:root`: si hay muchos componentes, el CSS se llena de bloques `@media` redundantes.
> - **Olvidar mobile-first**: si `:root` define los valores de escritorio y los breakpoints los reducen para móvil, el CSS está al revés.

## Notas relacionadas

- [[03 Ámbito y Herencia de Variables]] — cómo las variables se propagan a los descendientes.
- [[01 Sintaxis @media]] — la sintaxis de las media queries.
- [[07 Container Queries/01 @container y container-type]] — redefinir variables según el contenedor, no la ventana.
- [[06 Temas Dinámicos]] — otro patrón de redefinición de variables según contexto.
