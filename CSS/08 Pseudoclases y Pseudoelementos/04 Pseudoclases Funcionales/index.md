---
title: Pseudoclases Funcionales — Selectores que reciben selectores
aliases:
  - functional pseudo-classes
tags:
  - css
  - api/concepto
  - selectores
draft: false
order: 4
---

# Pseudoclases Funcionales

> [!definicion]
> Las pseudoclases **funcionales** reciben otro **selector** como argumento entre paréntesis: `:is()`, `:where()` (agrupar), `:has()` (seleccionar por descendiente/relación), `:not()` (excluir) y `:lang()` (por idioma). Son las más potentes y modernas, y resuelven problemas que antes exigían selectores larguísimos o JavaScript.

```css
:is(h1, h2, h3) { font-family: serif; }   /* agrupar */
.card:has(img) { padding: 0; }              /* padre según hijo */
li:not(.activo) { opacity: 0.6; }           /* excluir */
```

## Mapa de la subsección

- [[01 is()]] — agrupar selectores (con especificidad).
- [[02 where()]] — agrupar **sin** especificidad.
- [[03 has()]] — seleccionar según descendientes o relaciones ("selector de padre").
- [[04 not()]] — excluir elementos.
- [[05 lang()]] — seleccionar por idioma.

## La revolución de :has()

> [!tip] El "selector de padre" que cambió CSS
> Durante décadas, CSS solo seleccionaba **hacia abajo** (un hijo según su padre). [[03 has() | `:has()`]] permite por fin seleccionar **hacia arriba** (un padre según su hijo) y relaciones laterales:
> ```css
> .card:has(img) { }              /* tarjetas que CONTIENEN una imagen */
> label:has(+ input:required) { } /* labels seguidos de un campo requerido */
> form:has(:invalid) { }          /* formularios con algún campo inválido */
> ```
> Patrones que antes **solo** se podían hacer con JavaScript ahora son CSS puro. Es la incorporación más transformadora de los selectores modernos.

## is/where: agrupar, con o sin especificidad

> [!info] La diferencia clave entre :is() y :where()
> `:is()` y `:where()` hacen lo mismo (agrupar selectores para no repetir), pero difieren en la [[01 Especificidad/index | especificidad]]:
> - **`:is()`** toma la especificidad del selector **más alto** de su lista.
> - **`:where()`** tiene especificidad **cero** siempre.
>
> `:where()` es ideal para estilos base fáciles de sobrescribir; `:is()` para agrupar manteniendo el peso. Detalle en [[01 is()]] y [[02 where()]].

## Notas relacionadas

- [[03 has()]] — el más transformador.
- [[01 is()]] · [[02 where()]] — agrupar selectores.
- [[01 Especificidad/index]] — cómo afectan estas pseudoclases al peso.
