---
title: Cálculo de Especificidad CSS
aliases:
  - calcular especificidad
  - especificidad (A,B,C)
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 1
---

# Cálculo de Especificidad

> [!definicion]
> El **cálculo de especificidad** es el proceso por el que el navegador asigna un peso numérico a cada selector para resolver conflictos. El peso se expresa como tres componentes `(A, B, C)` donde A=IDs, B=clases/atributos/pseudoclases, C=elementos/pseudoelementos. La comparación es jerárquica, no aritmética.

```css
/* Ejercicio rápido — ¿cuál gana sobre <p class="intro" id="hero">? */
p               { color: black; }   /* (0,0,1) */
.intro          { color: blue;  }   /* (0,1,0) */
p.intro         { color: green; }   /* (0,1,1) */
#hero           { color: red;   }   /* (1,0,0) — gana */
#hero.intro     { color: pink;  }   /* (1,1,0) — gana si hubiera empate con otro ID */
```

## Las tres columnas en detalle

### Columna A — IDs

Cada selector de ID (`#nombre`) suma 1 al componente A. Tener un solo ID eleva el selector por encima de cualquier combinación de clases, sin importar cuántas sean.

```css
#header { }               /* (1,0,0) */
#header #nav { }          /* (2,0,0) */
```

### Columna B — Clases, atributos y pseudoclases

Cada clase (`.clase`), selector de atributo (`[attr]`) y pseudoclase (`:hover`, `:nth-child()`, `:not()`) suma 1 al componente B.

```css
.btn { }                           /* (0,1,0) */
.btn.activo { }                    /* (0,2,0) */
[type="submit"] { }                /* (0,1,0) */
.btn:hover { }                     /* (0,2,0) */
:nth-child(2) { }                  /* (0,1,0) */
:not(.excluido) { }                /* (0,1,0) — :not() no cuenta, su arg sí */
```

### Columna C — Elementos y pseudoelementos

Cada tipo de elemento (`div`, `p`, `input`) y cada pseudoelemento (`::before`, `::after`, `::placeholder`) suma 1 al componente C.

```css
p { }                              /* (0,0,1) */
ul li { }                          /* (0,0,2) */
p::first-line { }                  /* (0,0,2) */
div > span::before { }             /* (0,0,3) */
```

### Lo que no suma nada

```css
* { }                              /* (0,0,0) — universal */
div > p { }                        /* (0,0,2) — el combinador > no cuenta */
div + p { }                        /* (0,0,2) */
```

## Tabla de referencia rápida

| Selector | A | B | C | Peso |
|---|---|---|---|---|
| `*` | 0 | 0 | 0 | (0,0,0) |
| `p` | 0 | 0 | 1 | (0,0,1) |
| `div p` | 0 | 0 | 2 | (0,0,2) |
| `.clase` | 0 | 1 | 0 | (0,1,0) |
| `p.clase` | 0 | 1 | 1 | (0,1,1) |
| `.a.b` | 0 | 2 | 0 | (0,2,0) |
| `[type="text"]` | 0 | 1 | 0 | (0,1,0) |
| `:hover` | 0 | 1 | 0 | (0,1,0) |
| `#id` | 1 | 0 | 0 | (1,0,0) |
| `#id .clase` | 1 | 1 | 0 | (1,1,0) |
| `style=""` | — | — | — | supera todo |

## La regla de la jerarquía (no es suma decimal)

El error conceptual más frecuente: tratar `(0,0,10)` como si fuera 10 y `(0,1,0)` como si fuera 10 también, concluyendo que empatan. **No es así.**

`(0,1,0)` siempre supera a `(0,0,99)` porque la columna B es el siguiente nivel jerárquico; el valor de C se ignora hasta que A y B son iguales.

```css
/* 11 clases: (0,11,0) */
.a.b.c.d.e.f.g.h.i.j.k { color: blue; }

/* 1 ID: (1,0,0) — gana igualmente */
#x { color: red; }
```

## Recetas comunes

### Mantener la especificidad baja y plana

El objetivo en proyectos bien arquitecturados: que la mayoría de selectores vivan en `(0,1,0)` — una sola clase. Así cualquier modificador o estado (`(0,2,0)`) puede sobreescribirlos sin lucha.

```css
/* Bien: especificidad plana */
.card { }
.card--destacado { }  /* BEM: solo una clase más */

/* Mal: especificidad escalada */
.seccion .card { }
#main .seccion .card { }
```

### Reducir especificidad de un selector existente con :where()

Cuando necesitas usar un selector de alta especificidad pero no quieres que "infle" el cómputo:

```css
/* Sin :where() — (1,0,0) */
#nav a { color: blue; }

/* Con :where() en el ID — (0,0,1) */
:where(#nav) a { color: blue; }
```

### Igualar especificidad para el orden de origen

A veces la solución no es subir la especificidad del ganador sino bajar la del perdedor. Reescribir el selector con `:where()` o simplificar a una clase suele ser más limpio que añadir un ID.

## Cómo funciona por dentro

Internamente el navegador no almacena `(A,B,C)` como texto: construye un entero de múltiples bytes donde cada componente ocupa un campo separado. La comparación es una simple comparación de enteros, empezando por el campo más significativo. Por eso la jerarquía es estricta: ningún desbordamiento de C puede "carry" hacia B.

## Buenas prácticas

> [!tip] Mantener la especificidad predecible
> - Prefiere clases a IDs en selectores CSS: un ID en el HTML para JS o anchors, no para estilos.
> - Evita anidar selectores más de dos niveles; cada nivel añade C y acumula dependencias de estructura HTML.
> - Usa `@layer` para controlar prioridad sin subir especificidad.
> - Aprovecha `:where()` para envolver selectores de terceros o legados que inflan la especificidad.

## Errores comunes

> [!warning] Trampas
> - **Creer que 10 clases ganan a 1 ID**: la especificidad es jerárquica, no decimal.
> - **Usar IDs para estilos**: contaminan la especificidad y dificultan el override.
> - **Añadir clases o IDs para "subir"**: mejor atacar la raíz del conflicto.
> - **Olvidar que los combinadores no cuentan**: `div > p` tiene `(0,0,2)`, no `(0,0,3)`.

## Notas relacionadas

- [[index]] — introducción a la especificidad.
- [[02 Especificidad de Pseudoclases y Pseudoelementos]] — dónde caen `:hover`, `::before`, etc.
- [[03 Especificidad de is() y where()]] — `:is()` hereda; `:where()` anula.
- [[02 Importancia (!important)]] — el override que ignora la especificidad.
