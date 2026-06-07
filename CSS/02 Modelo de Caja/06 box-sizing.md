---
title: box-sizing — Qué incluye el width
aliases:
  - box-sizing
  - border-box
tags:
  - css
  - api/propiedad
  - layout
propiedad: box-sizing
grupo: box-model
valor_inicial: content-box
hereda: false
animable: false
draft: false
---

# box-sizing

> [!definicion]
> `box-sizing` controla **qué incluye el `width`/`height`**: solo el contenido (`content-box`, por defecto) o el contenido más el padding y el borde (`border-box`). Es uno de los ajustes más importantes de CSS, y casi todos los proyectos lo cambian a `border-box` globalmente.

```css
*, *::before, *::after { box-sizing: border-box; }
```

## El problema que resuelve

> [!warning] Con content-box, padding y borde se SUMAN al width
> Por defecto (`content-box`), `width` mide **solo el contenido**. El padding y el borde se añaden **encima**, así que el tamaño real es mayor del declarado:
> ```css
> .caja {
>   width: 200px;
>   padding: 20px;
>   border: 2px solid;
> }
> /* content-box: ancho REAL = 200 + 20·2 + 2·2 = 244px ❌ */
> ```
> Esto rompe los cálculos: una caja "de 200px" que en realidad ocupa 244 desborda los layouts y obliga a restar a mano.

## La solución: border-box

> [!tip] border-box hace que width sea el tamaño real
> Con `box-sizing: border-box`, `width` incluye padding y borde: el contenido se **encoge** para que el total sea exactamente el declarado:
> ```css
> .caja {
>   box-sizing: border-box;
>   width: 200px;
>   padding: 20px;
>   border: 2px solid;
> }
> /* border-box: ancho REAL = 200px exactos ✅ (el contenido ocupa 156px) */
> ```
> "200px significa 200px" es mucho más intuitivo y predecible.

## Los dos valores

| Valor | `width` incluye | Comportamiento |
|-------|-----------------|----------------|
| `content-box` | Solo el contenido | Padding y borde se suman (por defecto) |
| `border-box` | Contenido + padding + borde | El total es el `width` declarado |

(El margen **nunca** se incluye en `width`, con ningún valor.)

## El reset universal

> [!tip] El primer reset de todo proyecto
> Como `border-box` es tan superior, la práctica universal es aplicarlo a **todo** con el selector universal, incluyendo los pseudoelementos:
> ```css
> *, *::before, *::after {
>   box-sizing: border-box;
> }
> ```
> Esta es una de las primeras líneas de casi cualquier hoja de estilos moderna. Con ella, padding y bordes dejan de descuadrar los tamaños.

## Por qué se hereda con cuidado

`box-sizing` **no** se hereda por naturaleza. El reset universal lo aplica a cada elemento directamente. Existe una variante que lo hace heredable (`html { box-sizing: border-box } *{ box-sizing: inherit }`), útil para que componentes de terceros puedan optar por otro valor, pero la forma simple con `*` basta para la mayoría.

## Buenas prácticas

> [!tip] Recomendaciones
> - Aplica `box-sizing: border-box` globalmente con `*, *::before, *::after`.
> - Da por hecho `border-box` en el resto del CSS: simplifica todos los cálculos de tamaño.
> - Recuerda que el margen nunca entra en `width`, con ningún valor.
> - Si un componente de terceros asume `content-box`, acótalo localmente.

## Errores comunes

> [!warning] Trampas
> - **No poner el reset**: `width: 100%` + padding desborda con `content-box`.
> - **Olvidar `::before`/`::after`** en el reset: los pseudoelementos quedan en `content-box`.
> - **Mezclar** componentes con distinto `box-sizing` sin darse cuenta.

## Notas relacionadas

- [[01 width y height]] — lo que `box-sizing` reinterpreta.
- [[04 Selector Universal]] — el `*` del reset.
- [[01 Partes del Modelo de Caja]] — las capas que `border-box` agrupa.
