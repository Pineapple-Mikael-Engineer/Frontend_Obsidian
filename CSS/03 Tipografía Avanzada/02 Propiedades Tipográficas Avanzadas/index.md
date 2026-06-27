---
title: Propiedades Tipográficas Avanzadas — Kerning, ligaduras, fuentes variables
aliases:
  - advanced font properties
  - opentype features
tags:
  - css
  - api/concepto
  - tipografia
draft: false
order: 2
---

# Propiedades Tipográficas Avanzadas

> [!definicion]
> Un grupo de propiedades `font-*` da acceso a las capacidades **avanzadas** de las fuentes modernas: el ajuste de espaciado entre pares de letras (kerning), las funciones OpenType (ligaduras, versalitas, cifras), los ejes de las **fuentes variables** (peso, ancho, inclinación continuos) y el suavizado de renderizado.

```css
h1 {
  font-kerning: normal;
  font-feature-settings: "liga" 1;       /* ligaduras */
  font-variation-settings: "wght" 625;   /* peso exacto (variable font) */
}
```

## Mapa de la subsección

- [[01 font-kerning]] — el ajuste de espacio entre pares de letras.
- [[02 font-feature-settings]] — activar funciones OpenType de bajo nivel.
- [[03 font-variation-settings]] — los ejes de las fuentes variables.
- [[04 font-optical-sizing]] — ajuste óptico según el tamaño.
- [[05 font-stretch]] — la anchura (condensada/expandida).
- [[06 text-rendering]] — pista de prioridad al motor de renderizado.
- [[07 font-synthesis]] — controlar la síntesis de negrita/cursiva.

## Dos mundos: features OpenType y fuentes variables

> [!info] Activar lo que la fuente trae vs. moverse por un eje
> Estas propiedades caen en dos grupos:
> - **Funciones OpenType** (`font-feature-settings`, `font-kerning`, `font-variant-*`): **activan o desactivan** capacidades que la fuente ya incluye (ligaduras, kerning, versalitas). Son interruptores.
> - **Fuentes variables** (`font-variation-settings`, `font-optical-sizing`): mueven un valor a lo largo de un **eje continuo** (peso, ancho, inclinación, tamaño óptico) que la fuente define. Son deslizadores.

## Casi siempre hay un atajo de alto nivel

> [!tip] Prefiere las propiedades de alto nivel
> Muchas de estas capacidades tienen una propiedad de **alto nivel** más legible que `font-feature-settings`/`font-variation-settings`:
> - Para ligaduras → [[06 font-variant | `font-variant-ligatures`]].
> - Para peso → [[04 font-weight | `font-weight`]] (que en fuentes variables ya mueve el eje).
> - Para ancho → `font-width` (moderno) en vez de `font-variation-settings: "wdth"`.
>
> Usa los descriptores de bajo nivel solo para *features* que no tengan atajo.

## Notas relacionadas

- [[03 font-variation-settings]] — el corazón de las fuentes variables.
- [[06 font-variant]] — el atajo de alto nivel para variantes.
- [[01 Fuentes Web (@font-face)/index]] — cargar fuentes que soporten estas funciones.
