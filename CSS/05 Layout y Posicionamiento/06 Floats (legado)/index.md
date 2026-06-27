---
title: Floats (legado) — El layout antes de Flexbox y Grid
aliases:
  - floats
tags:
  - css
  - api/concepto
  - layout
draft: false
order: 6
---

# Floats (legado)

> [!definicion]
> El **float** saca un elemento del flujo y lo "flota" a la izquierda o derecha, permitiendo que el texto **lo rodee**. Era la herramienta de layout principal **antes** de Flexbox y Grid, usada (y abusada) para maquetar columnas enteras. Hoy su único uso legítimo es su función original: **envolver texto alrededor de una imagen**.

```css
.imagen { float: left; margin-right: 1rem; }   /* el texto rodea la imagen */
```

## Mapa de la subsección

- [[01 float]] — flotar un elemento a izquierda o derecha.
- [[02 clear]] — impedir que un elemento se ponga junto a un float.
- [[03 Clearfix]] — el hack para que un contenedor "contenga" sus floats.

## Su único uso legítimo hoy

> [!tip] Floats para envolver texto, no para layout
> El propósito **original** y único vigente de `float` es lo que hace bien: que un texto **fluya alrededor** de una imagen o un elemento (como en una revista). Para eso sigue siendo la herramienta correcta:
> ```css
> .articulo .foto { float: left; margin: 0 1rem 1rem 0; }
> /* el texto del artículo rodea la foto */
> ```
> Ni Flexbox ni Grid hacen esto (envolver texto): el float es insustituible aquí.

## Por qué NO usarlos para layout

> [!warning] Maquetar con floats es un antipatrón hoy
> Durante años se maquetaron columnas y layouts enteros con floats, a falta de algo mejor. Era **frágil y enrevesado**: requería el [[03 Clearfix | hack del clearfix]], cálculos de anchos manuales, y se rompía con facilidad. Hoy, para colocar elementos en columnas o rejillas, **Flexbox y Grid** son infinitamente mejores. Usar floats para layout en código nuevo es un error; solo aparecen en código heredado.

| Para… | NO uses floats, usa… |
|--------|----------------------|
| Columnas | Grid o Flexbox |
| Centrar | Flexbox/Grid |
| Barras de navegación | Flexbox |
| Envolver texto alrededor de una imagen | **float** (su uso correcto) |

## Por qué se documentan

Se incluyen en el temario para (1) reconocer y mantener **código heredado** que los use, y (2) saber usarlos para su **único caso vigente** (envolver texto). No para maquetar.

## Notas relacionadas

- [[01 float]] — la propiedad.
- [[03 Flexbox/index]] · [[04 CSS Grid/index]] — los sistemas que los reemplazaron para layout.
- [[03 Clearfix]] — el hack histórico asociado.
