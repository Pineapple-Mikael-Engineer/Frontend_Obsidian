---
title: Herencia Natural — Propiedades que se propagan al padre
aliases:
  - css inheritance
  - herencia
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 1
---

# Herencia Natural

> [!definicion]
> La **herencia** es el mecanismo por el que un elemento toma el valor de una propiedad de su **elemento padre** cuando no se le asigna uno propio. No todas las propiedades se heredan: solo las que tiene sentido propagar, sobre todo las **tipográficas**.

```css
body { color: #1e1e2e; font-family: system-ui; }
/* todo el texto del documento hereda este color y fuente */
```

## Qué se hereda y qué no

| Heredables (texto, tipografía) | No heredables (caja, layout) |
|--------------------------------|------------------------------|
| `color` | `margin`, `padding` |
| `font-family`, `font-size`, `font-weight`, `font-style` | `border`, `width`, `height` |
| `line-height`, `letter-spacing`, `text-align`, `text-indent` | `background`, `display`, `position` |
| `text-transform`, `white-space`, `visibility` | `box-shadow`, `overflow`, `z-index` |
| `list-style`, `cursor` | `top`/`left`, `float` |

> [!info] La lógica del reparto
> Las propiedades **tipográficas** se heredan porque es lo natural: si pones la fuente en `body`, quieres que todo el texto la tome sin repetirla. Las propiedades de **caja y layout** no se heredan porque sería absurdo: un hijo no debería heredar el `margin` o el `border` de su padre (se acumularían sin control). Cada propiedad declara en su especificación si hereda; las notas de propiedad de este vault lo indican en el frontmatter (`hereda: true/false`).

## El patrón: estilos base en body

> [!tip] Aprovecha la herencia para los estilos globales
> La herencia es lo que permite definir la tipografía base **una vez** y que se aplique a todo:
> ```css
> body {
>   font-family: system-ui, sans-serif;
>   font-size: 1rem;
>   line-height: 1.6;
>   color: #313244;
> }
> ```
> Todo el texto del documento hereda esto; solo sobrescribes donde cambie (títulos, código). Sin herencia, habría que repetir la fuente en cada elemento.

## Forzar la herencia donde no la hay

Algunas propiedades no heredables se pueden **forzar** a heredar con el valor [[02 Valores Especiales (inherit, initial, unset, revert) | `inherit`]]. Un caso clásico: que los botones e inputs hereden la fuente (los controles de formulario **no** la heredan por defecto):

```css
button, input, select, textarea {
  font: inherit;   /* heredan la tipografía del documento */
}
```

Sin esto, los controles usan la fuente del sistema, rompiendo la coherencia tipográfica.

## La herencia toma el valor calculado

> [!info] Se hereda el valor "computado", no el escrito
> Lo que se hereda es el valor **calculado** del padre, no el literal. Por eso `line-height` con número sin unidad (`1.6`) se comporta distinto de uno con unidad: el número se hereda como factor (cada hijo lo multiplica por su tamaño), mientras que `24px` se hereda ya resuelto a 24px. Es la razón de preferir [[07 line-height | `line-height` sin unidad]].

## Las variables CSS siempre heredan

> [!info] Las custom properties son heredables
> Las [[10 Variables CSS/index | variables CSS]] (`--mi-color`) **siempre** se heredan, sea cual sea la propiedad. Por eso definir `--color: x` en `:root` lo hace disponible en todo el árbol: es herencia pura.

## Buenas prácticas

> [!tip] Recomendaciones
> - Define los estilos de texto en `body`/`:root` y deja que se hereden.
> - Fuerza `font: inherit` en controles de formulario para coherencia.
> - Consulta si una propiedad hereda antes de esperar que se propague.
> - Usa variables CSS para "heredar" cualquier valor por el árbol.

## Errores comunes

> [!warning] Trampas
> - **Esperar que herede una propiedad de caja** (`margin`, `background`): no lo hacen.
> - **Olvidar `font: inherit`** en formularios: los controles no heredan la fuente.
> - **`line-height` con unidad**: se hereda fijo y rompe elementos de otro tamaño.

## Notas relacionadas

- [[02 Valores Especiales (inherit, initial, unset, revert)]] — controlar la herencia explícitamente.
- [[02 Cascada/index]] — qué regla gana antes de que entre la herencia.
- [[01 Definición y Uso (--var, var())]] — las variables, siempre heredables.
