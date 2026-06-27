---
title: z-index y Contexto de Apilamiento — El orden en profundidad
aliases:
  - z-index
  - stacking context
tags:
  - css
  - api/propiedad
  - layout
propiedad: z-index
grupo: layout
valor_inicial: auto
hereda: false
animable: true
draft: false
order: 7
---

# z-index y Contexto de Apilamiento

> [!definicion]
> `z-index` controla el **orden de apilado** en profundidad (eje Z): qué elemento queda **encima** de cuál cuando se superponen. Valores mayores van delante. Pero su funcionamiento depende del **contexto de apilamiento** (stacking context), el concepto que explica por qué a veces `z-index` "no funciona".

```css
.modal { position: fixed; z-index: 1000; }
.tooltip { position: absolute; z-index: 10; }
```

## z-index solo funciona en elementos posicionados

> [!warning] Necesita position (o flex/grid)
> `z-index` **se ignora** en elementos `static`. Solo aplica a elementos **posicionados** (relative/absolute/fixed/sticky) y a items de Flexbox/Grid. Si `z-index` no hace nada, lo primero a revisar es que el elemento tenga `position`:
> ```css
> .x { z-index: 10; }                      /* ❌ no hace nada (static) */
> .x { position: relative; z-index: 10; }  /* ✅ */
> ```

## El orden de apilado por defecto

Sin `z-index`, los elementos se apilan según: el orden del HTML (los posteriores van encima) y su tipo (los posicionados sobre los no posicionados). `z-index` permite **anular** ese orden.

## El contexto de apilamiento: la clave del misterio

> [!warning] z-index es relativo a su contexto, no global
> El malentendido más profundo de `z-index`: **no es global**. Un `z-index: 9999` dentro de un contexto de apilamiento **no puede** superar a un elemento de otro contexto que esté por encima. Los z-index solo se comparan **dentro del mismo contexto**.
>
> Un elemento crea un **nuevo contexto de apilamiento** (entre otros) cuando tiene:
> - `position` ≠ static **y** `z-index` ≠ auto.
> - `opacity` < 1.
> - `transform`, `filter`, `perspective`, `clip-path`, `mask` ≠ none.
> - `will-change` con ciertas propiedades.
> - `isolation: isolate`.
>
> Dentro de un contexto, los z-index de los hijos se ordenan **entre sí**, pero todo el grupo se apila como una unidad respecto al exterior.

## El bug clásico

> [!info] "Mi z-index altísimo no funciona"
> El síntoma típico: pones `z-index: 99999` a un menú y aun así queda **debajo** de otro elemento con `z-index: 2`. La causa: el menú está **dentro** de un contexto de apilamiento (creado por un ancestro con `opacity`, `transform`, etc.) que en conjunto está por debajo del otro elemento. Por mucho que subas el z-index **interno**, no escapa de su contexto. La solución pasa por mover el elemento fuera de ese contexto o ajustar el z-index del **contexto** padre.

## Gestionar z-index a escala

> [!tip] Usa una escala de z-index, no números al azar
> En proyectos grandes, repartir z-index arbitrarios (`9999`, `100000`) lleva al caos. Define una **escala** en variables, con rangos por capa:
> ```css
> :root {
>   --z-base: 0;
>   --z-dropdown: 10;
>   --z-sticky: 20;
>   --z-modal: 100;
>   --z-toast: 200;
> }
> .modal { z-index: var(--z-modal); }
> ```
> Da orden y evita la "guerra de z-index".

## Buenas prácticas

> [!tip] Recomendaciones
> - `z-index` requiere `position` (o flex/grid item).
> - Entiende los contextos de apilamiento: un z-index alto no escapa de su contexto.
> - Usa una **escala** de z-index en variables, no números mágicos.
> - Si un z-index "no funciona", busca un ancestro con `opacity`/`transform`/`filter` que cree un contexto.
> - `isolation: isolate` para crear un contexto a propósito y acotar el apilado.

## Errores comunes

> [!warning] Trampas
> - **`z-index` sin `position`**: se ignora.
> - **z-index altísimo que no escapa** de su contexto de apilamiento.
> - **Crear contextos sin querer** (un `opacity`/`transform` en un ancestro) que atrapan los hijos.
> - **Números mágicos** repartidos por todo el CSS.

## Notas relacionadas

- [[03 absolute]] · [[04 fixed]] — los elementos que más usan z-index.
- [[01 filter]] — un efecto que crea contexto de apilamiento.
- [[03 Modos de Mezcla (mix-blend-mode, background-blend-mode)]] — `isolation: isolate`.
