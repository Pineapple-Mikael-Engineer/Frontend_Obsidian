---
title: "Especificidad de :is() y :where()"
aliases:
  - "especificidad :is()"
  - "especificidad :where()"
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Especificidad de :is() y :where()

> [!definicion]
> `:is()` y `:where()` son pseudoclases funcionales que agrupan selectores. La diferencia clave es la especificidad: **`:is()` adopta el peso del argumento más específico** de su lista, mientras que **`:where()` siempre tiene especificidad cero**, ignorando la de sus argumentos. Esto los hace herramientas complementarias para controlar la especificidad con precisión.

```css
/* :is() hereda la especificidad más alta de sus argumentos */
:is(#nav, .menu) a { color: blue; }    /* (1,0,1) — por #nav */

/* :where() siempre aporta cero */
:where(#nav, .menu) a { color: blue; } /* (0,0,1) — solo a */
```

## :is() — especificidad del argumento más pesado

`:is()` toma la especificidad del argumento **más específico** de su lista, aunque ese argumento no coincida con ningún elemento real. Esto es deliberado: el navegador no puede saber en tiempo de análisis cuál argumento coincidirá.

```css
/* :is(h1, h2, h3) — argumento más pesado es un elemento (C=1) */
:is(h1, h2, h3) { margin: 0; }         /* (0,0,1) */

/* :is(.primario, .secundario) — una clase (B=1) */
:is(.primario, .secundario) { }        /* (0,1,0) */

/* :is(#hero, .seccion) — ID contamina (A=1) */
:is(#hero, .seccion) p { }             /* (1,0,1) */
/* TRAMPA: aunque el #hero no exista en el DOM, la especificidad sigue siendo (1,0,1) */
```

### La trampa del ID "fantasma"

Si incluyes un ID en la lista de `:is()`, la especificidad del selector completo sube a A=1, **aunque el elemento que coincida lo haga por la clase o el elemento, no por el ID**.

```css
/* Ambas reglas apuntan a <p class="texto">: */
.texto p { color: blue; }              /* (0,1,1) */
:is(#fantasma, .texto) p { color: red; } /* (1,0,1) — gana, aunque #fantasma no exista */
```

Este comportamiento es coherente con el spec pero contraintuitivo. La solución: no mezclar IDs y clases en el mismo `:is()` si el resultado importa para el cascade.

## :where() — especificidad siempre cero

`:where()` es idéntico a `:is()` en cuanto a los elementos que selecciona, pero su especificidad es **siempre `(0,0,0)`**, independientemente de lo que contenga.

```css
/* :where() con ID dentro — sigue siendo (0,0,0) */
:where(#hero) p { color: blue; }       /* (0,0,1) — solo el p */

/* Útil para resets de bajo peso */
:where(h1, h2, h3, h4, h5, h6) {
  font-weight: bold;
  margin: 0;
}
/* Cualquier clase puede sobrescribir esto sin luchar */
.titulo { font-weight: 400; }          /* (0,1,0) — gana fácilmente */
```

## Cuándo usar cada uno

| Situación | Herramienta | Por qué |
|---|---|---|
| Agrupar variantes de un componente | `:is()` | La especificidad del componente se mantiene igual que si escribieras cada selector por separado |
| Resets y estilos base de biblioteca | `:where()` | Fácil de sobrescribir sin `!important` |
| Reducir un selector legado de alta especificidad | `:where()` | Envuelve el selector pesado para neutralizarlo |
| Selector con `:hover` en múltiples elementos | `:is()` | Legible y equivalente a repetir |

## Recetas comunes

### Agrupar variantes sin cambiar la especificidad

```css
/* Sin :is() — tres reglas con (0,1,1) */
.btn:hover,
.link:hover,
.tag:hover { opacity: 0.8; }

/* Con :is() — una regla con (0,1,1): .btn/.link/.tag son clases (B=1) + :hover (B+1) */
:is(.btn, .link, .tag):hover { opacity: 0.8; }
```

### Reset de bajo peso para una biblioteca

```css
/* La biblioteca no sabe qué clases usará el consumidor.
   Con :where(), cualquier clase del proyecto puede sobrescribir sin !important */
:where(button, input, select, textarea) {
  font-family: inherit;
  font-size: inherit;
  box-sizing: border-box;
}
```

### Reducir la especificidad de un selector heredado

```css
/* Un selector legado que infla la especificidad: (1,0,1) */
#sidebar a { color: blue; }

/* Envolverlo para que el resto del CSS pueda sobrescribirlo: (0,0,1) */
:where(#sidebar) a { color: blue; }
```

## :has() también usa el argumento

`:has()` sigue la misma regla que `:is()`: adopta la especificidad del argumento más pesado de su lista.

```css
/* :has(.destacado) — B=1 por .destacado + C=1 por section */
section:has(.destacado) { }           /* (0,2,0) */

/* :has(> p) — C=1 por p + C=1 por div */
div:has(> p) { }                      /* (0,0,2) */
```

## Cómo funciona por dentro

El algoritmo del navegador para `:is()` y `:where()` es el mismo en cuanto a la coincidencia de elementos. La única diferencia está en el paso de cálculo de especificidad: para `:is()`, se itera la lista de argumentos y se toma el máximo; para `:where()`, ese paso devuelve siempre `(0,0,0)`. La separación es limpia en el spec (Selectors Level 4) y es lo que hace que `:where()` sea ideal como "zero-specificity grouping".

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:where()` en resets y estilos base: cualquier selector de proyecto los sobreescribirá sin esfuerzo.
> - Usa `:is()` para reducir verbosidad en listas de variantes sin cambiar la especificidad esperada.
> - Evita mezclar IDs con clases en `:is()` si el resultado puede confundir: el ID "contamina" aunque no matchee.
> - Documenta en comentarios de equipo cuando uses `:where()` deliberadamente para especificidad cero: no es obvio para quien lea el CSS.

## Errores comunes

> [!warning] Trampas
> - **Asumir que `:is(#id, .clase)` tiene especificidad de clase**: el ID contamina incluso si no coincide.
> - **Usar `:where()` cuando la especificidad importa para cascada**: si necesitas que el selector gane, `:is()` es el correcto.
> - **Confundir `:is()` con `:matches()`**: `:matches()` es el nombre antiguo, con soporte obsoleto; úsase `:is()`.

## Notas relacionadas

- [[01 Cálculo de Especificidad]] — la mecánica base de los tres componentes.
- [[02 Especificidad de Pseudoclases y Pseudoelementos]] — dónde caen `:hover`, `::before`, etc.
- [[04 Pseudoclases Funcionales/02 where()]] — `:where()` como selector.
- [[04 Pseudoclases Funcionales/01 is() y matches()]] — `:is()` como selector.
