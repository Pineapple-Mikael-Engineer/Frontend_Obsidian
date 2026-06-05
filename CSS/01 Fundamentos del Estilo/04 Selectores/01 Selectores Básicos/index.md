---
title: Selectores Básicos — Tipo, clase, id, universal
aliases:
  - basic selectors
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Selectores Básicos

> [!definicion]
> Los selectores básicos apuntan a los elementos por su característica más directa: el **tipo** de elemento, su **clase**, su **id** o **todos** ellos (universal). Son los bloques con los que se construye cualquier selector más complejo.

```css
p          { }   /* tipo: todos los <p> */
.aviso     { }   /* clase: class="aviso" */
#cabecera  { }   /* id: id="cabecera" */
*          { }   /* universal: todos los elementos */
```

## Los cinco básicos

| Selector | Sintaxis | Apunta a | Especificidad | Nota |
|----------|----------|----------|---------------|------|
| Tipo | `p` | Todos los elementos de ese tipo | Baja | [[01 Selector de Tipo]] |
| Clase | `.nombre` | Elementos con esa clase | Media | [[02 Selector de Clase]] |
| Id | `#nombre` | El elemento con ese id | Alta | [[03 Selector de ID]] |
| Universal | `*` | Todos los elementos | Ninguna | [[04 Selector Universal]] |
| Agrupación | `a, b, c` | Varios a la vez | (la de cada uno) | [[05 Agrupación de Selectores]] |

## El más usado: la clase

> [!tip] La clase es el caballo de batalla
> De los cinco, el **selector de clase** es el que más se usa en la práctica: tiene una especificidad manejable, es reutilizable (muchos elementos pueden compartir una clase) y desacopla el estilo del tipo de elemento concreto. Las metodologías modernas ([[01 BEM | BEM]], utility-first) se construyen casi enteramente sobre clases.

## Mapa de la subsección

- [[01 Selector de Tipo]] — por nombre de elemento (`h1`, `p`, `div`).
- [[02 Selector de Clase]] — por `class` (`.boton`), el más versátil.
- [[03 Selector de ID]] — por `id` (`#menu`), único y de alta especificidad.
- [[04 Selector Universal]] — `*`, todos los elementos.
- [[05 Agrupación de Selectores]] — `a, b` para compartir un bloque.

## Combinar tipo y clase

Los básicos se pueden encadenar sin espacio para exigir **varias** condiciones en el mismo elemento:

```css
p.aviso      { }   /* <p> que ADEMÁS tiene class="aviso" */
.btn.activo  { }   /* elemento con AMBAS clases btn y activo */
```

(Con espacio cambia el significado a "descendiente", tema de [[02 Combinadores/index | combinadores]].)

## Notas relacionadas

- [[02 Selector de Clase]] — el más importante.
- [[02 Combinadores/index]] — relacionar elementos entre sí.
- [[01 Cálculo de Especificidad]] — el peso de cada básico.
