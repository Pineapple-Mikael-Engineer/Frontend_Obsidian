---
title: Margin Collapse — El colapso de márgenes verticales
aliases:
  - margin collapse
  - colapso de márgenes
tags:
  - css
  - api/concepto
  - layout
draft: false
---

# Margin Collapse

> [!definicion]
> El **colapso de márgenes** (margin collapse) es un comportamiento de CSS por el que los márgenes **verticales** de elementos adyacentes (o de padre e hijo) **se fusionan** en uno solo —el **mayor** de ellos— en lugar de sumarse. Es una de las rarezas más confusas del modelo de caja, y conocerlo evita muchas sorpresas de espaciado.

```css
.a { margin-bottom: 30px; }
.b { margin-top: 20px; }
/* entre .a y .b NO hay 50px, sino 30px (el mayor) */
```

## Los tres casos donde ocurre

| Caso | Descripción |
|------|-------------|
| **Hermanos adyacentes** | El `margin-bottom` de uno y el `margin-top` del siguiente colapsan |
| **Padre e hijo** | El `margin-top` del primer hijo "sale" y colapsa con el del padre |
| **Caja vacía** | El `margin-top` y `margin-bottom` de un elemento sin contenido colapsan entre sí |

## Solo márgenes verticales, solo en flujo normal

> [!info] Las condiciones del colapso
> El colapso ocurre **únicamente**:
> - En márgenes **verticales** (block), no horizontales (inline).
> - En el **flujo normal** del documento.
>
> **No** colapsan los márgenes en **Flexbox**, **Grid**, ni en elementos posicionados (`absolute`, `float`). Por eso, en un contenedor flex/grid, los márgenes se comportan de forma "normal" (se suman), lo que para muchos es más predecible.

## El colapso padre-hijo, el más sorprendente

> [!warning] El margin del hijo "se escapa" del padre
> El caso que más confunde: si un hijo tiene `margin-top` y el padre no tiene **nada que los separe** (ni borde, ni padding, ni contenido antes), ese margen **atraviesa** al padre y se aplica por fuera de él:
> ```css
> .padre { background: #eee; }
> .hijo  { margin-top: 40px; }
> /* el espacio de 40px aparece ARRIBA del padre, no dentro */
> ```
> Resultado: el fondo del padre no incluye ese hueco superior, lo que parece un bug. La solución es poner algo entre ellos: un `padding-top`, un `border`, o `overflow` distinto de `visible`.

## Cómo evitarlo o controlarlo

> [!tip] Formas de prevenir el colapso
> | Técnica | Efecto |
> |---------|--------|
> | Usar Flexbox/Grid | El colapso **no ocurre** ahí |
> | `padding` o `border` en el padre | Separa los márgenes, evita el colapso padre-hijo |
> | `overflow: auto`/`hidden` en el padre | Crea un contexto que evita el colapso |
> | `display: flow-root` en el padre | Crea un contexto de formato de bloque limpio |
> | Margen en **un solo** sentido | El patrón `* + *` o solo `margin-bottom` evita colapsos entre hermanos |

## La estrategia del margen único

> [!tip] Aplica margen en una sola dirección
> Para no pelear con el colapso, una práctica común es aplicar márgenes **siempre en el mismo sentido** (p. ej. solo `margin-bottom`, o el patrón [[03 Hermano Adyacente (+) | "lobotomized owl"]] `* + * { margin-top }`). Así el espaciado entre elementos es predecible y el colapso deja de ser un problema:
> ```css
> .flujo > * + * { margin-block-start: 1rem; }
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Conoce el colapso para no sorprenderte con espaciados "que faltan".
> - Aplica márgenes en un solo sentido (`* + *` o solo `margin-bottom`).
> - En layout moderno (Flexbox/Grid con `gap`), el colapso no aparece: prefiere `gap` para espaciar.
> - Para el caso padre-hijo, usa `padding`/`border` o `display: flow-root`.

## Errores comunes

> [!warning] Trampas
> - **Esperar que dos márgenes se sumen**: colapsan al mayor.
> - **El margen del hijo "escapa" del padre**: parece un hueco fuera del fondo del padre.
> - **Asumir colapso en Flexbox/Grid**: ahí no ocurre (los márgenes se suman).

## Notas relacionadas

- [[05 Margin]] — la propiedad cuyo comportamiento vertical colapsa.
- [[07 Espaciado (gap)]] — `gap` en flex/grid, sin colapso, más predecible.
- [[03 Hermano Adyacente (+)]] — el patrón `* + *` para espaciar sin colapso.
