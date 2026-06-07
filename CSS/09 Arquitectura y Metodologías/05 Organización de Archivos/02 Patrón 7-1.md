---
title: "Patrón 7-1 para Organización de CSS"
aliases:
  - patrón 7-1
  - 7-1 pattern
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Patrón 7-1

> [!definicion]
> El **patrón 7-1** es una estructura de organización de archivos CSS/Sass que divide los estilos en **7 carpetas temáticas** y los importa todos desde **1 archivo de entrada** (`main.scss` o `main.css`). Es el estándar de facto en proyectos Sass medianos y grandes, y sus principios aplican igualmente a CSS puro con `@import` y `@layer`.

```
sass/
├── abstracts/      ← variables, mixins, funciones (sin CSS real)
├── base/           ← resets, tipografía, elementos HTML
├── components/     ← botones, tarjetas, modales, nav...
├── layout/         ← header, footer, grid, sidebar...
├── pages/          ← estilos específicos de página
├── themes/         ← variantes de color / modo oscuro
├── vendors/        ← CSS de terceros
└── main.scss       ← punto de entrada (solo @use/@forward/@import)
```

## Las 7 carpetas

### 1. abstracts (o utilities)

Contiene todo lo que **no genera CSS por sí mismo**: variables Sass, mixins, funciones, placeholders. En CSS puro equivale a los design tokens (variables CSS en `:root`).

```scss
/* abstracts/_variables.scss */
$color-primary: #3b82f6;
$font-base: system-ui, sans-serif;
$space-4: 1rem;
$radius-md: 6px;

/* abstracts/_mixins.scss */
@mixin flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

@mixin responsive($breakpoint) {
  @if $breakpoint == md {
    @media (min-width: 768px) { @content; }
  } @else if $breakpoint == lg {
    @media (min-width: 1024px) { @content; }
  }
}
```

### 2. base

Estilos globales sin clase: resets, normalización, estilos por defecto para elementos HTML. Equivale a la categoría Base de SMACSS.

```scss
/* base/_reset.scss */
*, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }
body { font-family: $font-base; line-height: 1.6; color: #111; }

/* base/_typography.scss */
h1 { font-size: clamp(1.75rem, 4vw, 2.5rem); line-height: 1.2; }
h2 { font-size: clamp(1.5rem, 3vw, 2rem); }
p { max-width: 65ch; }
```

### 3. components

El corazón del proyecto: un archivo por cada componente de UI reutilizable. Equivale a Module en SMACSS y a los bloques BEM.

```
components/
├── _btn.scss
├── _card.scss
├── _nav.scss
├── _modal.scss
├── _form.scss
├── _badge.scss
└── _toast.scss
```

Cada archivo contiene el bloque BEM completo (bloque + elementos + modificadores):

```scss
/* components/_btn.scss */
.btn {
  display: inline-flex;
  align-items: center;
  padding: $space-4 * 0.5 $space-4;
  border-radius: $radius-md;
  cursor: pointer;
  transition: background 0.2s;

  &--primario { background: $color-primary; color: white; }
  &--secundario { background: transparent; border: 1px solid $color-primary; color: $color-primary; }
  &--grande { padding: $space-4 * 0.75 $space-4 * 1.5; font-size: 1.125rem; }
}
```

### 4. layout

Estilos de la estructura mayor de la página: cabecera, pie, barra lateral, grid principal. No contiene componentes — solo los contenedores.

```
layout/
├── _header.scss
├── _footer.scss
├── _sidebar.scss
├── _grid.scss
└── _nav-principal.scss
```

```scss
/* layout/_grid.scss */
.l-contenedor { max-width: 72rem; margin-inline: auto; padding-inline: 1rem; }
.l-dos-columnas { display: grid; grid-template-columns: 1fr 20rem; gap: 2rem; }
.l-centrado { display: grid; place-items: center; min-height: 100svh; }
```

### 5. pages

Estilos específicos de una página concreta que no son reutilizables en otras páginas. Si hay pocos, pueden ir en `components/`; si hay muchos, merecen su carpeta.

```
pages/
├── _home.scss
├── _about.scss
└── _checkout.scss
```

```scss
/* pages/_home.scss */
.home__hero { min-height: 80svh; background: linear-gradient(to right, #1e40af, #3b82f6); }
.home__features { display: grid; grid-template-columns: repeat(auto-fit, minmax(min(18rem, 100%), 1fr)); gap: 2rem; }
```

### 6. themes

Variantes de color, modo oscuro, o temas de marca. En proyectos modernos se implementan con variables CSS en `data-theme` en lugar de clases Sass:

```scss
/* themes/_dark.scss */
[data-theme="dark"] {
  --color-bg: #1a1a2e;
  --color-text: #e2e8f0;
  --color-border: #334155;
}

/* themes/_high-contrast.scss */
[data-theme="high-contrast"] {
  --color-primary: #0000ff;
  --color-bg: #ffffff;
  --color-text: #000000;
}
```

### 7. vendors

CSS de terceros: normalize.css, librerías de componentes, iconos. Se importan sin modificar (si se modifican, pierden el beneficio de ser actualizables).

```
vendors/
├── _normalize.scss
└── _algolia-search.scss
```

## El archivo main.scss

El punto de entrada declara el orden de importación explícitamente. En Sass moderno (`@use`/`@forward`) el orden importa menos porque Sass gestiona las dependencias, pero en CSS puro el orden de `@import` es la cascada.

```scss
/* main.scss */
// Abstracts primero (no generan CSS, deben estar disponibles para todos)
@use 'abstracts/variables' as *;
@use 'abstracts/mixins' as *;

// El orden genera la cascada: base → layout → components → pages → themes
@use 'base/reset';
@use 'base/typography';

@use 'layout/grid';
@use 'layout/header';
@use 'layout/footer';

@use 'components/btn';
@use 'components/card';
@use 'components/nav';

@use 'pages/home';
@use 'pages/about';

@use 'themes/dark';
@use 'vendors/normalize';
```

## 7-1 en CSS puro con @layer

Sin Sass, el patrón 7-1 aplica igual, usando `@import` con `@layer` para controlar la cascada:

```css
/* main.css */
@layer vendors, base, layout, components, pages, themes, utilities;

@import "vendors/normalize.css" layer(vendors);
@import "base/reset.css" layer(base);
@import "base/typography.css" layer(base);
@import "layout/grid.css" layer(layout);
@import "layout/header.css" layer(layout);
@import "components/btn.css" layer(components);
@import "components/card.css" layer(components);
@import "pages/home.css" layer(pages);
@import "themes/dark.css" layer(themes);
```

## Cuándo usar el patrón 7-1 completo

El patrón completo (las 7 carpetas) es adecuado para proyectos medianos y grandes con Sass y múltiples desarrolladores. Para proyectos pequeños o con CSS puro, una versión simplificada (3-4 carpetas) es más práctica:

```
styles/
├── main.css
├── base.css
├── components/
│   ├── btn.css
│   └── card.css
└── utilities.css
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Mantén `abstracts/` limpio: no debe generar ninguna línea de CSS compilado.
> - En `components/`, un archivo por componente; en `layout/`, un archivo por sección de página.
> - `pages/` solo si hay estilos que no pueden generalizarse; si pueden, van en `components/`.
> - En Sass moderno, prefiere `@use` sobre `@import`: evita conflictos de nombres y es el futuro del lenguaje.
> - Actualiza `vendors/` con el gestor de paquetes, no manualmente.

## Errores comunes

> [!warning] Trampas
> - **Todo en `components/`**: sin separar layout de componentes, el orden de cascada se vuelve impredecible.
> - **Estilos en `abstracts/`**: si hay CSS real en las variables o mixins, hay un error de diseño.
> - **Modificar archivos de `vendors/`**: se pierden al actualizar la dependencia.
> - **No establecer el orden de capas al inicio**: añadir archivos sin orden explícito rompe la cascada cuando el proyecto crece.

## Notas relacionadas

- [[index]] — la organización de archivos en contexto.
- [[01 Estructura Modular]] — el fundamento del que parte el patrón 7-1.
- [[04 Metodologías de Nomenclatura/02 SMACSS]] — las categorías SMACSS mapean a las carpetas del 7-1.
- [[03 Capas (@layer)/01 Definición y Orden de Capas]] — cómo las @layer formalizan el orden del 7-1.
