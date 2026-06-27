---
title: Herencia y Valores — Cómo se propagan y resuelven los valores
aliases:
  - inheritance and values
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 7
---

# Herencia y Valores

> [!definicion]
> Dos mecanismos gobiernan cómo un elemento obtiene el valor de una propiedad cuando no se le asigna directamente: la **herencia** (tomar el valor del padre) y los **valores especiales** (`inherit`, `initial`, `unset`, `revert`) que controlan ese comportamiento de forma explícita.

```css
body { color: #1e1e2e; }   /* se hereda a todo el texto interior */
.reset { all: revert; }    /* devuelve todo a los estilos del navegador */
```

## Mapa de la sección

- [[01 Herencia Natural]] — qué propiedades se heredan y cuáles no, y por qué.
- [[02 Valores Especiales (inherit, initial, unset, revert)]] — controlar la herencia y el reseteo.

## De dónde sale el valor de una propiedad

> [!info] La cadena de resolución
> Cuando el navegador necesita el valor de una propiedad para un elemento, lo resuelve por pasos:
> 1. ¿Hay una regla que se lo asigne (ganadora en la [[02 Cascada/index | cascada]])? → ese valor.
> 2. Si no, ¿la propiedad **se hereda**? → toma el valor calculado del padre.
> 3. Si no se hereda (o es la raíz) → usa el **valor inicial** de la propiedad.
>
> La herencia y los valores especiales actúan en los pasos 2 y 3.

## Qué se hereda y qué no

| Suelen heredarse | No suelen heredarse |
|------------------|---------------------|
| Propiedades de **texto**: `color`, `font-*`, `line-height`, `text-align` | Propiedades de **caja**: `margin`, `padding`, `border`, `width` |
| `visibility`, `cursor`, `list-style` | `background`, `display`, `position` |

La lógica: las propiedades **tipográficas** se heredan (un párrafo debe tomar la fuente del documento), mientras que las de **layout/caja** no (un hijo no debería heredar el margen de su padre). Detalle en [[01 Herencia Natural]].

## Notas relacionadas

- [[01 Herencia Natural]] — el reparto entre heredables y no heredables.
- [[02 Cascada/index]] — cómo se elige la regla ganadora antes de heredar.
- [[10 Variables CSS/index]] — las custom properties **siempre** se heredan.
