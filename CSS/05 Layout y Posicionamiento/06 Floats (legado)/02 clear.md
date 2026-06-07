---
title: clear — Impedir ponerse junto a un float
aliases:
  - clear
tags:
  - css
  - api/propiedad
  - layout
propiedad: clear
grupo: layout
valor_inicial: none
hereda: false
animable: false
draft: false
---

# clear

> [!definicion]
> `clear` impide que un elemento se coloque **junto a** un float: lo empuja **debajo** del float (a la izquierda, derecha o ambos). Sirve para "cortar" el efecto de envoltura y forzar que un elemento empiece en una línea limpia bajo los floats anteriores.

```css
.siguiente { clear: both; }   /* empieza debajo de cualquier float anterior */
```

## Valores

| Valor | Efecto |
|-------|--------|
| `none` | Puede ponerse junto a floats (por defecto) |
| `left` | Se coloca debajo de los floats a la izquierda |
| `right` | Debajo de los floats a la derecha |
| `both` | Debajo de **cualquier** float (el más usado) |

## Para qué sirve

> [!info] Cortar la envoltura de texto
> Cuando un elemento está flotado y el texto lo rodea, a veces quieres que un elemento concreto **no** se ponga al lado, sino que empiece limpio debajo. `clear` lo logra:
> ```css
> .articulo img { float: left; }
> .articulo .pie-seccion { clear: both; }   /* este empieza bajo la imagen, no a su lado */
> ```
> El pie de sección ya no se mete junto a la imagen, sino que baja hasta superarla.

## clear: both, el más usado

`clear: both` es el valor habitual: ignora si el float es izquierdo o derecho y simplemente coloca el elemento **debajo de todos** los floats anteriores. Es la forma de "reiniciar" el flujo tras una zona con floats.

## La base del clearfix

> [!info] clear es el ingrediente del clearfix
> El [[03 Clearfix | hack del clearfix]] (que hace que un contenedor "contenga" la altura de sus floats) se basa en `clear`: añade un pseudoelemento con `clear: both` al final del contenedor, forzándolo a extenderse hasta bajo los floats. Hoy hay alternativas mejores (`display: flow-root`), pero `clear` sigue siendo la pieza histórica.

## Buenas prácticas

> [!tip] Recomendaciones
> - `clear: both` para que un elemento empiece limpio debajo de los floats anteriores.
> - Úsalo junto con `float` en su caso vigente (envolver texto), no para layout.
> - Para "contener" floats en un contenedor, prefiere `display: flow-root` al clearfix.
> - En código nuevo, evita la combinación float/clear para maquetar.

## Errores comunes

> [!warning] Trampas
> - **Esperar que `clear` afecte al elemento flotado**: actúa sobre el elemento al que se lo pones, no sobre el float.
> - **Usarlo para maquetar** en vez de Flexbox/Grid.

## Notas relacionadas

- [[01 float]] — el float cuyo efecto `clear` corta.
- [[03 Clearfix]] — el hack que usa `clear`.
- [[03 Flexbox/index]] — la alternativa moderna que no necesita clear.
