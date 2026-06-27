---
title: Relativas a Fuente — em, rem, ch, ex
aliases:
  - font-relative units
  - rem
  - em
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 2
---

# Relativas a Fuente (em, rem, ch, ex)

> [!definicion]
> Estas unidades se calculan **en función del tamaño de fuente**. Las dos clave son `rem` (relativa a la fuente **raíz**) y `em` (relativa a la fuente del **elemento**). Son la base de un diseño **escalable y accesible**, porque crecen con la preferencia de tamaño de texto del usuario.

```css
font-size: 1.5rem;   /* 1.5 × tamaño de fuente raíz */
padding: 1em;        /* 1 × tamaño de fuente de este elemento */
width: 60ch;         /* ancho de 60 caracteres "0" */
```

## Las unidades

| Unidad | Relativa a | Uso típico |
|--------|-----------|------------|
| `rem` | `font-size` de `<html>` (raíz) | Tipografía y espaciado (recomendada) |
| `em` | `font-size` del elemento actual | Espaciado proporcional al texto local |
| `ch` | Ancho del carácter `0` de la fuente | Anchos de medida de lectura |
| `ex` | Altura de la `x` minúscula | Raro; ajustes tipográficos finos |

## rem: la unidad estrella

> [!tip] rem para casi todo
> `rem` se calcula siempre respecto a la fuente **raíz** (`<html>`), que por defecto es **16px**. Es predecible (no depende del anidamiento) y **escala con la preferencia del usuario**:
> ```css
> :root { font-size: 16px; }   /* o sin declararlo: 16px por defecto */
> h1   { font-size: 2rem; }    /* = 32px, pero escala si el usuario sube la fuente */
> .caja { padding: 1.5rem; }   /* = 24px, coherente con la tipografía */
> ```
> Si el usuario configura su navegador a 20px, todos los `rem` crecen proporcionalmente. Por eso es la unidad recomendada para tamaños de fuente y espaciado.

## em: relativa al elemento, y se acumula

`em` se calcula respecto al `font-size` del **propio elemento**, lo que la hace útil para espaciados proporcionales al texto local (un padding que crece con el tamaño del botón):

```css
.boton {
  font-size: 1.2rem;
  padding: 0.5em 1em;   /* el padding escala con el tamaño del botón */
}
```

> [!warning] em se compone en cascada
> El gran riesgo de `em` en `font-size`: se **acumula** con el anidamiento. Si un `.menu` tiene `font-size: 1.2em` y dentro hay otro `.submenu` con `1.2em`, el submenú es 1.2 × 1.2 = 1.44 veces la fuente del abuelo. Anidamientos profundos producen tamaños inesperados ("compounding"). Por eso, para `font-size`, `rem` (que no se acumula) suele ser más seguro; `em` es ideal para propiedades **distintas** de `font-size` (padding, margin) que deban escalar con el texto local.

## ch: anchos de lectura

`ch` equivale al ancho del carácter `0`, útil para limitar la **medida** (longitud de línea) a un rango legible:

```css
p { max-width: 65ch; }   /* ~65 caracteres por línea, óptimo para leer */
```

La tipografía recomienda 45–75 caracteres por línea; `ch` lo expresa directamente.

## Recetas de tamaño raíz

> [!info] El truco del 62.5%
> Un patrón clásico ponía `html { font-size: 62.5%; }` (= 10px), de modo que `1rem = 10px` y los cálculos fueran fáciles (`1.6rem` = 16px). Funciona, pero **reduce** el tamaño base que el usuario configuró, así que hoy se desaconseja: es mejor dejar la raíz en su valor por defecto (respetando la preferencia) y pensar en múltiplos de 16.

## Buenas prácticas

> [!tip] Recomendaciones
> - `rem` para `font-size` y espaciado: escalable, accesible, predecible.
> - `em` para padding/margin que deban crecer con el texto **local** del componente.
> - `ch` para limitar la longitud de línea de los párrafos.
> - Evita `em` en `font-size` anidado (se acumula); evita reducir la raíz con el truco del 62.5%.

## Errores comunes

> [!warning] Trampas
> - **`em` en `font-size` anidado**: tamaños que se multiplican inesperadamente.
> - **Todo en `px`** en vez de `rem`: pierdes la escalabilidad accesible.
> - **Confundir `rem` (raíz) con `em` (elemento)**: dan resultados distintos al anidar.

## Notas relacionadas

- [[01 Unidades Absolutas (px, pt, cm)]] — `px`, que no escala.
- [[03 font-size]] — la propiedad que estas unidades dimensionan.
- [[07 min(), max(), clamp()]] — combinar `rem` con límites fluidos.
