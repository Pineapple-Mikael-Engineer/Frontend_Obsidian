---
title: Especificidad CSS
aliases:
  - especificidad
  - specificity
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Especificidad CSS

> [!definicion]
> La **especificidad** es el peso que el navegador asigna a un selector para decidir qué declaración aplica cuando dos reglas apuntan al mismo elemento. Se expresa como tres números `(A, B, C)` que forman un valor de precedencia: no es aritmética decimal, es jerárquica.

## Cómo se calcula

Cada selector suma a una de tres columnas:

| Columna | Cuenta | Ejemplos |
|---|---|---|
| A — IDs | Selectores de ID | `#nav`, `#logo` |
| B — Clases / atributos / pseudoclases | Clases, atributos y pseudoclases | `.btn`, `[type="text"]`, `:hover` |
| C — Elementos / pseudoelementos | Tipo de elemento y pseudoelementos | `div`, `p`, `::before` |

El selector con la columna de mayor orden no vacía gana. Solo si A es igual se compara B; solo si B también es igual se compara C. Es jerarquía, no suma: `(0,1,0)` gana siempre a `(0,0,99)`.

```css
/* (0,0,1) — un elemento */
p { color: black; }

/* (0,1,0) — una clase */
.texto { color: blue; }   /* gana */

/* (0,1,1) — clase + elemento */
p.texto { color: green; } /* gana sobre .texto */

/* (1,0,0) — un ID */
#intro { color: red; }    /* gana sobre todo lo anterior */
```

## Lo que no cuenta

- El selector universal `*` tiene especificidad `(0,0,0)`.
- Los combinadores (`>`, `+`, `~`, ` `) no añaden especificidad.
- Los pseudoelementos (`::before`, `::after`) cuentan como C (=1), igual que un elemento.
- `:not()`, `:is()`, `:has()` no añaden especificidad por sí mismos, pero sus argumentos sí (ver nota sobre `:is()` y `:where()`).

## El estilo en línea

Un estilo en línea (`style="color: red"`) supera a cualquier selector (equivale a `(1,0,0,0)` en la notación extendida de 4 columnas). Solo `!important` puede superarlo, y `!important` en línea es casi imposible de sobrescribir.

## La trampa de la especificidad creciente

El anti-patrón más frecuente: añadir especificidad para arreglar colisiones, lo que obliga a más especificidad en el futuro, en espiral.

```css
/* Inicial */
.btn { background: blue; }

/* Colisión — se añade ID para ganar */
#sidebar .btn { background: green; }

/* Otro equipo necesita sobreescribir — debe ir aún más lejos */
#sidebar #footer .btn { background: red; }
```

La solución es mantener la especificidad baja y uniforme. Con `@layer` (ver nota correspondiente) se puede controlar el orden sin escalar la especificidad.

## Herramienta mental: la "torre de especificidad"

Pensar en especificidad como una torre de tres pisos: solo cuando el piso superior (A) está vacío se mira el siguiente (B), y así. Añadir un ID al selector sube al piso más alto e ignora todos los pisos inferiores del competidor, sin importar cuántas clases acumule.

## Notas relacionadas

- [[01 Cálculo de Especificidad]] — ejemplos detallados y la regla de los tres números.
- [[02 Especificidad de Pseudoclases y Pseudoelementos]] — dónde caen `:hover`, `::before`, etc.
- [[03 Especificidad de is() y where()]] — la diferencia entre las funciones selectoras.
- [[02 Cascada/index]] — especificidad es solo uno de los factores de la cascada.
