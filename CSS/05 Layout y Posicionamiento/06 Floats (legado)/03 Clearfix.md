---
title: Clearfix — Contener floats en un contenedor
aliases:
  - clearfix
  - flow-root
tags:
  - css
  - api/concepto
  - layout
draft: false
---

# Clearfix

> [!definicion]
> El **clearfix** es un hack histórico para que un contenedor "contenga" la altura de sus elementos flotados. Sin él, un contenedor cuyos hijos son todos floats **colapsa** (altura 0), porque los floats salen del flujo. Hoy `display: flow-root` lo resuelve de forma limpia, pero el clearfix sigue apareciendo en código heredado.

```css
/* La versión moderna y recomendada */
.contenedor { display: flow-root; }
```

## El problema: el contenedor colapsa

> [!warning] Un contenedor con solo floats tiene altura 0
> Como los [[01 float | floats]] salen del flujo, un contenedor cuyos hijos son **todos** floats no "ve" su altura y **colapsa** a 0px. Su fondo y borde no envuelven los floats, que se salen por abajo:
> ```css
> .galeria { background: #eee; }
> .galeria img { float: left; }
> /* ❌ .galeria tiene altura 0: el fondo gris no se ve, las imágenes desbordan */
> ```

## La solución moderna: display: flow-root

> [!tip] flow-root, el clearfix sin hacks
> La forma **correcta y moderna** de hacer que un contenedor contenga sus floats es `display: flow-root`, que crea un contexto de formato de bloque limpio:
> ```css
> .galeria { display: flow-root; }   /* contiene los floats, sin pseudoelementos ni hacks */
> ```
> Es una línea, sin efectos secundarios. **Es la opción recomendada** en código nuevo que use floats.

## El clearfix clásico (heredado)

> [!info] El hack que verás en código antiguo
> Antes de `flow-root`, se usaba el "clearfix": un pseudoelemento con [[02 clear | `clear: both`]] al final del contenedor, que lo fuerza a extenderse hasta bajo los floats:
> ```css
> .clearfix::after {
>   content: "";
>   display: block;
>   clear: both;
> }
> ```
> Funciona, pero es más verboso y menos obvio que `flow-root`. Conviene **reconocerlo** en código heredado.

## Otras formas de contener floats

| Técnica | Nota |
|---------|------|
| `display: flow-root` | **Recomendada**: limpia, sin efectos secundarios |
| Clearfix (`::after` + `clear`) | Heredada, funciona |
| `overflow: auto`/`hidden` | Contiene floats, pero puede recortar/crear scroll |

`overflow: hidden` también contiene floats (crea un contexto de bloque), pero como efecto secundario recorta el contenido desbordado, así que `flow-root` es preferible.

## En perspectiva

> [!info] Un problema casi extinto
> Todo este asunto (clearfix, contenedores colapsados) era central cuando se maquetaba con floats. Con **Flexbox y Grid**, los contenedores **siempre** contienen a sus hijos correctamente: no hay floats, no hay colapso, no hace falta clearfix. Por eso este tema es, sobre todo, para entender y mantener código antiguo.

## Buenas prácticas

> [!tip] Recomendaciones
> - En código nuevo con floats, usa `display: flow-root` para contenerlos.
> - Reconoce el clearfix clásico (`::after` + `clear: both`) en código heredado.
> - Evita `overflow: hidden` como clearfix (recorta el contenido).
> - Mejor aún: usa Flexbox/Grid y olvídate del problema por completo.

## Errores comunes

> [!warning] Trampas
> - **Contenedor colapsado** por no contener los floats.
> - **`overflow: hidden`** como clearfix que recorta contenido sin querer.
> - **Seguir usando floats** para layout cuando Grid/Flexbox lo evitan todo.

## Notas relacionadas

- [[01 float]] · [[02 clear]] — las piezas del problema.
- [[03 Flexbox/index]] · [[04 CSS Grid/index]] — los sistemas que hacen el clearfix innecesario.
- [[07 Margin Collapse]] — `flow-root` también ayuda con el colapso de márgenes.
