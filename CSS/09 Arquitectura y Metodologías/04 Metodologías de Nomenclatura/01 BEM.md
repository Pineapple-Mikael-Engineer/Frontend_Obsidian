---
title: BEM — Block Element Modifier
aliases:
  - BEM
  - block element modifier
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 1
---

# BEM — Block Element Modifier

> [!definicion]
> **BEM** (Block Element Modifier) es una metodología de nomenclatura que organiza las clases CSS en tres categorías: **Block** (componente autónomo), **Element** (parte del bloque) y **Modifier** (variante o estado). La convención de nombres es `bloque__elemento--modificador`, y su objetivo es que cada clase sea autoexplicativa y que los estilos no dependan del contexto del DOM.

```html
<!-- BEM en acción -->
<div class="card card--destacado">
  <img class="card__imagen" src="...">
  <div class="card__contenido">
    <h2 class="card__titulo">Título</h2>
    <p class="card__descripcion">Texto</p>
    <a class="card__enlace card__enlace--primario" href="#">Leer más</a>
  </div>
</div>
```

```css
.card { border: 1px solid #e5e7eb; border-radius: 8px; overflow: hidden; }
.card--destacado { border-color: #3b82f6; box-shadow: 0 4px 12px rgb(0 0 0 / 0.1); }
.card__imagen { width: 100%; display: block; }
.card__contenido { padding: 1rem; }
.card__titulo { font-size: 1.25rem; font-weight: 600; margin-bottom: 0.5rem; }
.card__descripcion { color: #6b7280; margin-bottom: 1rem; }
.card__enlace { color: #3b82f6; text-decoration: none; }
.card__enlace--primario { font-weight: 600; }
```

## Los tres conceptos

### Block (Bloque)

Un bloque es un **componente independiente y reutilizable**. Tiene significado por sí mismo. El nombre del bloque debe ser semántico (describe su función, no su apariencia).

```css
/* Buenos nombres de bloque */
.menu { }
.btn { }
.card { }
.formulario-busqueda { }

/* Malos nombres de bloque (describen apariencia, no función) */
.azul { }
.grande { }
.flotante-derecha { }
```

### Element (Elemento)

Un elemento es una **parte del bloque que no tiene sentido fuera de él**. Se nombra con doble guion bajo: `bloque__elemento`.

```css
.nav__lista { list-style: none; display: flex; gap: 1rem; }
.nav__elemento { }
.nav__enlace { color: inherit; text-decoration: none; }

.formulario__campo { display: block; width: 100%; }
.formulario__etiqueta { font-size: 0.875rem; color: #374151; }
.formulario__error { color: #ef4444; font-size: 0.75rem; }
```

Los elementos no se anidan en el CSS usando el selector de hijo: `.nav__lista__elemento` (tres niveles) es incorrecto en BEM. Si necesitas anidar, el elemento hijo sigue siendo hijo del bloque: `.nav__subelemento`.

```html
<!-- Correcto: el elemento siempre pertenece al bloque, aunque el DOM esté anidado -->
<nav class="nav">
  <ul class="nav__lista">
    <li class="nav__elemento">
      <a class="nav__enlace" href="#">Inicio</a>
    </li>
  </ul>
</nav>
```

### Modifier (Modificador)

Un modificador describe una **variante o estado** del bloque o elemento. Se nombra con doble guion: `bloque--modificador` o `bloque__elemento--modificador`.

```css
/* Modificadores de bloque */
.btn { padding: 0.5rem 1rem; border-radius: 4px; }
.btn--primario { background: #3b82f6; color: white; }
.btn--secundario { background: white; border: 1px solid #3b82f6; color: #3b82f6; }
.btn--grande { padding: 0.75rem 1.5rem; font-size: 1.125rem; }
.btn--deshabilitado { opacity: 0.5; pointer-events: none; }

/* Modificadores de elemento */
.menu__enlace--activo { font-weight: bold; color: #3b82f6; border-bottom: 2px solid currentColor; }
```

Los modificadores siempre se usan junto al bloque o elemento base, nunca solos:

```html
<!-- Correcto -->
<button class="btn btn--primario btn--grande">Enviar</button>

<!-- Incorrecto: --primario sin la clase base .btn -->
<button class="btn--primario">Enviar</button>
```

## Por qué BEM mantiene la especificidad baja

Una de las ventajas más importantes de BEM es que todos los selectores son de una sola clase: especificidad `(0,1,0)`. No hay IDs, no hay selectores de hijo, no hay anidación. Esto hace que cualquier override sea predecible:

```css
/* BEM — toda la especificidad es (0,1,0) */
.btn { }
.btn--primario { }
.btn--grande { }

/* Comparado con CSS sin metodología */
.sidebar .menu li a { }    /* (0,2,2) — difícil de sobrescribir */
#nav .link:hover { }       /* (1,1,1) — casi imposible sin !important */
```

## Recetas comunes

### Tarjeta con variantes

```html
<article class="tarjeta tarjeta--horizontal">
  <figure class="tarjeta__imagen">
    <img src="..." alt="...">
  </figure>
  <div class="tarjeta__cuerpo">
    <span class="tarjeta__categoria tarjeta__categoria--tecnologia">Tecnología</span>
    <h2 class="tarjeta__titulo">Artículo</h2>
    <p class="tarjeta__extracto">Resumen breve...</p>
  </div>
</article>
```

```css
.tarjeta { display: flex; flex-direction: column; border: 1px solid #e5e7eb; border-radius: 8px; }
.tarjeta--horizontal { flex-direction: row; }
.tarjeta__imagen { flex-shrink: 0; }
.tarjeta__imagen img { display: block; width: 100%; height: 100%; object-fit: cover; }
.tarjeta__cuerpo { padding: 1rem; }
.tarjeta__categoria { font-size: 0.75rem; text-transform: uppercase; letter-spacing: 0.05em; }
.tarjeta__categoria--tecnologia { color: #3b82f6; }
.tarjeta__titulo { font-size: 1.25rem; margin-block: 0.5rem; }
.tarjeta__extracto { color: #6b7280; }
```

### Formulario completo

```html
<form class="form">
  <div class="form__grupo">
    <label class="form__etiqueta" for="email">Correo</label>
    <input class="form__campo form__campo--error" id="email" type="email">
    <p class="form__mensaje form__mensaje--error">El formato no es válido</p>
  </div>
  <button class="btn btn--primario" type="submit">Enviar</button>
</form>
```

## BEM y estados dinámicos: mezcla con SMACSS

Una limitación de BEM puro es que los modificadores son clases estáticas. Para estados dinámicos controlados con JavaScript (activo, abierto, cargando), muchos equipos adoptan el prefijo de estado de SMACSS: `is-` o `has-`:

```html
<nav class="nav">
  <a class="nav__enlace is-activo" href="#">Inicio</a>
</nav>
```

```css
/* El modificador BEM (estático) */
.nav__enlace--activo { }

/* El estado SMACSS (dinámico, añadido por JS) */
.nav__enlace.is-activo { font-weight: bold; }
```

La segunda forma tiene especificidad `(0,2,0)`, lo que permite que el estado JS sobrescriba al modificador BEM sin lucha.

## Cuándo usar BEM (y cuándo no)

BEM encaja bien en sistemas de componentes (React, Vue, Web Components) donde cada componente tiene su propio espacio de nombres. No es ideal para estilos globales (tipografía base, resets), donde las pseudoclases y los tipos de elemento son más naturales. Tampoco escala bien si el diseño no está basado en componentes discretos.

## Cómo funciona por dentro

BEM no es una herramienta ni una especificación: es una convención de texto. El navegador no conoce BEM; solo ve clases. La convención de doble guion bajo y doble guion convierte el nombre de la clase en una mini-documentación: cualquier desarrollador puede deducir la jerarquía `.formulario__campo--error` → campo de error dentro del componente formulario, sin ver el HTML.

## Buenas prácticas

> [!tip] Recomendaciones
> - Nómbralo por función, no por apariencia: `.tarjeta--destacada` es mejor que `.tarjeta--azul`.
> - No anides más de dos niveles: `bloque__elemento--modificador` es el máximo; `bloque__nivel1__nivel2` es una señal de que el componente es demasiado complejo.
> - Combina BEM con `is-` / `has-` para los estados controlados con JavaScript.
> - Usa una clase por elemento: si necesitas múltiples clases BEM en el mismo elemento, revisa si el componente tiene demasiadas responsabilidades.

## Errores comunes

> [!warning] Trampas
> - **Nesting infinito**: `.bloque__elem1__elem2__elem3` — no existe en BEM; todos los elementos son hijos del bloque.
> - **Modificador sin bloque base**: `.btn--primario` sin `.btn` — el modificador no funciona solo.
> - **Nombres descriptivos de apariencia**: `.titulo--rojo`, `.contenedor--grande` — al cambiar el diseño, el nombre miente.
> - **Selector de hijo en CSS**: `.card .card__titulo` — rompe la especificidad plana de BEM.

## Notas relacionadas

- [[index]] — las metodologías en contexto.
- [[02 SMACSS]] — organización por categorías, complemento habitual de BEM.
- [[04 Utility-First]] — el extremo opuesto: una clase por propiedad.
- [[01 Especificidad/index]] — por qué la especificidad plana de BEM importa.
