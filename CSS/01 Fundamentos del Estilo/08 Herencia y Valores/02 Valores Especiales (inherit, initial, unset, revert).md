---
title: Valores Especiales — inherit, initial, unset, revert
aliases:
  - css-wide keywords
  - inherit initial unset revert
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Valores Especiales (inherit, initial, unset, revert)

> [!definicion]
> Cuatro palabras clave funcionan en **cualquier** propiedad CSS y controlan de dónde toma su valor: `inherit` (del padre), `initial` (el valor por defecto de la especificación), `unset` (uno u otro según si hereda) y `revert` (vuelve a los estilos del navegador o del usuario). Son las *CSS-wide keywords*.

```css
.x { color: inherit; }    /* el color del padre */
.y { color: initial; }    /* el valor inicial de color (negro) */
.z { all: revert; }       /* deshace los estilos de autor */
```

## Las cuatro palabras

| Palabra | Toma el valor… |
|---------|----------------|
| `inherit` | Del **elemento padre** (fuerza la herencia, herede o no la propiedad) |
| `initial` | El **valor inicial** que define la especificación de la propiedad |
| `unset` | `inherit` si la propiedad **es heredable**, `initial` si **no** |
| `revert` | El que tendría por los estilos del **navegador/usuario** (deshace los del autor) |

## inherit: forzar la herencia

`inherit` hace que una propiedad tome el valor del padre **aunque no se herede** por naturaleza. El caso típico: que los controles de formulario hereden la tipografía.

```css
button { font: inherit; color: inherit; }
```

## initial: el valor de fábrica

`initial` devuelve la propiedad a su valor **inicial según la especificación** (no el del navegador, ni el del padre). Cuidado: el valor inicial de la especificación a veces no es lo "esperado" (`display: initial` es `inline`, no `block`).

> [!warning] initial no es "lo que pone el navegador"
> El valor inicial es el de la **especificación**, que puede diferir del estilo por defecto del **navegador**. Por ejemplo, el navegador pone `<div>` en `display: block`, pero `display: initial` lo devuelve a `inline` (el inicial de la propiedad). Para "deshacer y volver al estilo del navegador", usa `revert`, no `initial`.

## unset: heredar o resetear, automático

`unset` actúa como `inherit` para propiedades heredables (como `color`) y como `initial` para las no heredables (como `border`). Es útil para "limpiar" un valor dejando que cada propiedad haga lo natural.

## revert: deshacer los estilos de autor

> [!tip] revert para volver al estilo del navegador
> `revert` es el más intuitivo para "deshacer **mi** CSS": devuelve la propiedad al valor que tendría por los estilos del navegador (y del usuario), ignorando las reglas del autor. Combinado con [[02 Importancia (!important) | la propiedad `all`]], es potente para resetear un componente:
> ```css
> .contenido-externo { all: revert; }
> /* deshace todos los estilos de autor: se ve como HTML "crudo" */
> ```

## La propiedad all

Estas palabras brillan con la propiedad abreviada `all`, que las aplica a **todas** las propiedades a la vez:

| Uso | Efecto |
|-----|--------|
| `all: initial` | Todo a sus valores iniciales de especificación |
| `all: unset` | Todo heredado o inicial según corresponda |
| `all: revert` | Todo al estilo del navegador (el reset más limpio) |

## Buenas prácticas

> [!tip] Recomendaciones
> - `inherit` para forzar herencia donde no la hay (fuentes en formularios).
> - `revert` (no `initial`) para volver al aspecto por defecto del navegador.
> - `unset`/`all: unset` para limpiar estilos dejando que cada propiedad decida.
> - `all: revert` en un contenedor para neutralizar estilos heredados de un widget externo.

## Errores comunes

> [!warning] Trampas
> - **Esperar que `initial` dé el estilo del navegador**: da el de la especificación (`display: initial` = `inline`).
> - **Confundir `unset` con `revert`**: `unset` mira si hereda; `revert` deshace al autor.
> - **`all: initial`** esperando un reset visual razonable: puede dar resultados extraños (usa `revert`).

## Notas relacionadas

- [[01 Herencia Natural]] — qué propiedades heredan (base de `unset`).
- [[02 Cascada/index]] — los orígenes (navegador, usuario, autor) que `revert` distingue.
- [[01 Palabras Clave de Color]] — `inherit`/`initial` aplicadas al color.
