---
title: SMACSS — Scalable and Modular Architecture for CSS
aliases:
  - SMACSS
  - scalable modular architecture css
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# SMACSS

> [!definicion]
> **SMACSS** (Scalable and Modular Architecture for CSS) es una metodología creada por Jonathan Snook que divide los estilos en **cinco categorías**: Base, Layout, Module, State y Theme. A diferencia de BEM, SMACSS no impone una convención de nombres estricta: su aportación principal es la **categorización semántica** de las reglas CSS, que mejora la organización y reduce las colisiones.

## Las cinco categorías

### 1. Base

Estilos por defecto aplicados a **elementos HTML sin clase**. Son resets, normalizaciones y los valores predeterminados del proyecto. No se usan selectores de clase ni de ID — solo tipos de elemento, combinadores y pseudoclases estructurales.

```css
/* SMACSS — Base */
*, *::before, *::after { box-sizing: border-box; }
body { font-family: system-ui, sans-serif; line-height: 1.6; color: #111; }
h1, h2, h3, h4, h5, h6 { line-height: 1.2; }
a { color: #2563eb; }
a:hover { text-decoration: none; }
img { max-width: 100%; display: block; }
```

### 2. Layout

Estilos que definen la **estructura mayor de la página**: el encabezado, el pie, la barra lateral, el contenido principal. SMACSS sugiere el prefijo `l-` o `layout-` para las clases de layout.

```css
/* SMACSS — Layout */
.l-encabezado { display: flex; align-items: center; justify-content: space-between; padding: 1rem; }
.l-contenido-principal { max-width: 72rem; margin-inline: auto; padding: 2rem; }
.l-barra-lateral { width: 20rem; flex-shrink: 0; }
.l-dos-columnas { display: grid; grid-template-columns: 1fr 20rem; gap: 2rem; }
```

Los layout son únicos en la página (o muy pocos). Por eso SMACSS permite IDs en esta categoría, aunque la tendencia moderna es usar clases para mantener la especificidad plana.

### 3. Module

Los **componentes reutilizables**: tarjetas, botones, formularios, navegación, modales. Es la categoría más grande en cualquier proyecto. No llevan prefijo especial.

```css
/* SMACSS — Module */
.card { border: 1px solid #e5e7eb; border-radius: 8px; padding: 1rem; }
.btn { display: inline-flex; align-items: center; padding: 0.5rem 1rem; border-radius: 4px; }
.nav { display: flex; gap: 1rem; list-style: none; }
.modal { position: fixed; inset: 0; display: flex; align-items: center; justify-content: center; }
```

### 4. State

Estilos que representan **estados dinámicos** de la UI, generalmente activados o desactivados por JavaScript. SMACSS establece el prefijo `is-` o `has-` para estas clases.

```css
/* SMACSS — State */
.is-activo { opacity: 1; visibility: visible; }
.is-oculto { display: none; }
.is-cargando { cursor: wait; pointer-events: none; }
.is-invalido { border-color: #ef4444; }
.has-error { color: #ef4444; }
.has-dropdown-abierto .dropdown { display: block; }
```

Las clases de estado no son semánticamente de un componente concreto: `.is-activo` puede aplicarse a un enlace de nav, a una pestaña, a un botón. Esta transversalidad es intencional.

### 5. Theme

Estilos que permiten variaciones de **apariencia global**: modo oscuro, temas de color, variantes de marca. SMACSS sugiere el prefijo `theme-`.

```css
/* SMACSS — Theme */
.theme-oscuro { background: #1a1a2e; color: #e2e8f0; }
.theme-oscuro .card { background: #16213e; border-color: #0f3460; }
.theme-oscuro a { color: #60a5fa; }
```

En proyectos modernos, los temas se implementan habitualmente con variables CSS en `:root` o con `data-theme`, por lo que esta categoría se solapam con la nota de variables CSS.

## Organización de archivos en SMACSS

SMACSS sugiere un archivo por categoría (o por módulo dentro de la categoría de módulos):

```
styles/
├── base.css
├── layout.css
├── modules/
│   ├── btn.css
│   ├── card.css
│   ├── nav.css
│   └── modal.css
├── states.css
└── theme.css
```

## SMACSS y BEM: complementarios

SMACSS define **qué tipo de regla** es cada bloque de CSS; BEM define **cómo nombrar las clases** dentro de los módulos. En la práctica, se usan juntos:

```css
/* Archivo: modules/card.css — usando BEM dentro de la categoría Module de SMACSS */
.card { }
.card--destacado { }
.card__imagen { }
.card__titulo { }

/* Archivo: states.css — usando prefijo SMACSS para estados dinámicos */
.is-activo { }
.is-cargando { }
```

Los estados SMACSS (`is-activo`) se aplican junto a las clases BEM en el HTML:

```html
<a class="nav__enlace is-activo" href="#">Inicio</a>
```

## Ventajas sobre CSS sin estructura

| Sin SMACSS | Con SMACSS |
|---|---|
| Todo mezclado en un archivo | Separado por tipo de regla |
| Estados con clases ad-hoc (`.activo`, `.seleccionado`) | Convención unificada (`is-`) |
| Layout y módulos mezclados | Layout separado, baja especificidad |
| Imposible saber si un estilo es global o de componente | Categoría clara por nombre de archivo/prefijo |

## Cuándo usar SMACSS

SMACSS es especialmente útil en proyectos medianos y grandes donde el equipo necesita acordar convenciones sin adoptar un framework de utilidades. Es flexible: no obliga a cambiar toda la nomenclatura existente, solo a categorizar los nuevos estilos. En proyectos con componentes web o React, BEM + prefijos de estado SMACSS es una combinación habitual.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `is-` y `has-` para todos los estados controlados por JavaScript.
> - Separa layout de módulos: los estilos de layout no deberían estar dentro de los archivos de componente.
> - Incluye la categorización en comentarios de bloque si no usas un archivo por módulo.
> - En proyectos con `@layer`, mapea cada categoría SMACSS a una capa: `@layer base, layout, modules, utilities;`.

## Errores comunes

> [!warning] Trampas
> - **Mezclar estados con modificadores**: `.btn--activo` (BEM, estático) vs `.is-activo` (SMACSS, dinámico) tienen propósitos distintos.
> - **Poner estilos de layout dentro de módulos**: el componente card no debería saber dónde vive en el layout.
> - **Ignorar la categoría Base**: dejar los resets y los estilos de elemento en el mismo archivo que los componentes complica la cascada.

## Notas relacionadas

- [[index]] — las metodologías en contexto.
- [[01 BEM]] — nomenclatura de componentes que se combina bien con SMACSS.
- [[04 Utility-First]] — alternativa donde el "state" y el "theme" se expresan con clases de utilidad.
- [[05 Organización de Archivos/index]] — cómo SMACSS se traduce a una estructura de archivos.
