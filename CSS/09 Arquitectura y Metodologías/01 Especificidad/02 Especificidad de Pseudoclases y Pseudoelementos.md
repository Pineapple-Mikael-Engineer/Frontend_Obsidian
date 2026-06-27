---
title: Especificidad de Pseudoclases y Pseudoelementos
aliases:
  - especificidad pseudoclases
  - especificidad pseudoelementos
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 2
---

# Especificidad de Pseudoclases y Pseudoelementos

> [!definicion]
> Las **pseudoclases** (`:hover`, `:nth-child()`, `:focus`) contribuyen a la columna B de la especificidad, igual que una clase. Los **pseudoelementos** (`::before`, `::after`, `::placeholder`) contribuyen a la columna C, igual que un elemento. Conocer esto evita sorpresas al combinar selectores con pseudo-selectores.

```css
a:hover    { color: red;  }   /* (0,1,1) — B+1 por :hover, C+1 por a */
a::before  { content: ""; }   /* (0,0,2) — C+1 por a, C+1 por ::before */
```

## Pseudoclases — columna B

Todas las pseudoclases suman 1 al componente B, independientemente de si son simples (`:hover`) o funcionales (`:nth-child(2n)`). La excepción son `:not()`, `:is()` y `:has()`, que se tratan de forma especial.

```css
:hover          { }   /* (0,1,0) */
:focus          { }   /* (0,1,0) */
:nth-child(2)   { }   /* (0,1,0) */
:first-child    { }   /* (0,1,0) */
:checked        { }   /* (0,1,0) */
:enabled        { }   /* (0,1,0) */

/* Combinadas */
a:hover:focus   { }   /* (0,2,1) */
```

### La excepción: :not(), :is(), :has(), :matches()

Estas pseudoclases funcionales **no** añaden su propio peso — pero los selectores dentro de sus paréntesis sí cuentan. El peso adoptado es el del argumento más específico de la lista.

```css
/* :not(.clase) — (0,1,0): .clase aporta B=1, :not no aporta nada */
a:not(.externo) { }      /* (0,1,1) */

/* :is(h1, h2, h3) — (0,0,1): el más específico de los args es un elemento */
:is(h1, h2, h3).titulo  { }   /* (0,1,1) */

/* :is(#nav, .menu) — (1,0,0): el más específico es un ID */
:is(#nav, .menu) a { }  /* (1,0,1) */
```

`:where()` es la excepción a la excepción: siempre contribuye `(0,0,0)`, eliminando el peso de sus argumentos (ver nota específica).

## Pseudoelementos — columna C

Los pseudoelementos añaden 1 al componente C. Desde CSS3, se escriben con doble dos puntos `::` para distinguirlos de las pseudoclases, aunque `:before` y `:after` (simple) siguen siendo válidos por compatibilidad.

```css
::before         { }   /* (0,0,1) */
::after          { }   /* (0,0,1) */
::placeholder    { }   /* (0,0,1) */
::first-line     { }   /* (0,0,1) */
::first-letter   { }   /* (0,0,1) */
::marker         { }   /* (0,0,1) */
::backdrop       { }   /* (0,0,1) */

/* Combinados con un elemento */
p::first-line    { }   /* (0,0,2): p=C1, ::first-line=C1 */

/* Combinados con clase */
.intro::before   { }   /* (0,1,1): .intro=B1, ::before=C1 */
```

## Tabla comparativa

| Pseudo-selector | Columna | Valor |
|---|---|---|
| `:hover` | B | +1 |
| `:focus` | B | +1 |
| `:nth-child()` | B | +1 |
| `:first-child` | B | +1 |
| `:not()` | — | 0 (arg cuenta) |
| `:is()` | — | 0 (arg más alto) |
| `:where()` | — | siempre 0 |
| `::before` | C | +1 |
| `::after` | C | +1 |
| `::placeholder` | C | +1 |
| `::first-line` | C | +1 |
| `::marker` | C | +1 |

## Ejemplos resueltos

```css
/* ¿Cuál gana sobre <a class="enlace" href="#">? */
a              { color: black; }       /* (0,0,1) */
.enlace        { color: blue;  }       /* (0,1,0) — gana sobre a */
a.enlace       { color: green; }       /* (0,1,1) — gana sobre .enlace */
a.enlace:hover { color: red;   }       /* (0,2,1) — gana al hacer hover */

/* Pseudoelemento no compite con el elemento base */
a::before      { content: "→ "; }     /* (0,0,2) — diferente "objetivo" */
```

El pseudoelemento `::before` no compite con `a` porque apunta a un objeto distinto (el contenido generado), no al elemento en sí. La especificidad solo importa cuando ambas reglas apuntan exactamente al mismo "objeto".

## Cómo funciona por dentro

El navegador trata los pseudoelementos como si fueran un sub-elemento del nodo al que se aplican. Por eso tienen especificidad C (como un elemento) y no interfieren con los estilos del elemento padre. Las pseudoclases, en cambio, son condiciones del estado del elemento, no un sub-elemento, por lo que se agrupan en B (como las clases de estado o atributo).

## Buenas prácticas

> [!tip] Recomendaciones
> - Recuerda que `:hover` en un selector sube la columna B: `.btn:hover` tiene `(0,2,0)`, lo que importa si otro selector intenta sobrescribirlo.
> - Prefiere `::before`/`::after` con clase (`.icono::before`) sobre elemento (`.icono p::before`) para mantener especificidad más baja.
> - Usa `:where()` cuando necesites agrupar selectores sin añadir especificidad (ideal para resets y bibliotecas).

## Errores comunes

> [!warning] Trampas
> - **Creer que `:hover` no cuenta**: sí suma B=1; `a:hover` tiene `(0,1,1)` y gana a `a.clase` con `(0,1,1)` por orden de origen.
> - **Mezclar `:` y `::`**: los pseudoelementos con `:` simple funcionan por compatibilidad, pero conceptualmente son distintos de las pseudoclases.
> - **Olvidar que `:is(#id, .clase)` adopta el peso del ID**: un argumento de alta especificidad contamina todo el `:is()`.

## Notas relacionadas

- [[01 Cálculo de Especificidad]] — la mecánica de los tres componentes.
- [[03 Especificidad de is() y where()]] — `:is()` hereda; `:where()` siempre es cero.
- [[01 Pseudoclases de Interacción/index]] — `:hover`, `:focus`, `:active`.
- [[05 Pseudoelementos/index]] — `::before`, `::after`, `::marker`, etc.
