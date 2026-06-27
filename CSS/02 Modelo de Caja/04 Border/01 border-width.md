---
title: border-width — Grosor del borde
aliases:
  - border-width
tags:
  - css
  - api/propiedad
  - layout
propiedad: border-width
grupo: box-model
valor_inicial: medium
hereda: false
animable: true
draft: false
order: 1
---

# border-width

> [!definicion]
> `border-width` define el **grosor** del borde. Acepta una longitud o tres palabras clave (`thin`, `medium`, `thick`). Por sí solo no muestra nada: necesita un [[02 border-style | `border-style`]] distinto de `none`.

```css
.caja { border-width: 2px; border-style: solid; }
```

## Valores

| Valor | Grosor |
|-------|--------|
| Longitud | `1px`, `0.125rem`, etc. (lo más usado) |
| `thin` | Borde fino (~1px) |
| `medium` | Medio (~3px) — valor inicial |
| `thick` | Grueso (~5px) |

Las palabras clave dan grosores aproximados que **varían entre navegadores**, así que para precisión se usa una longitud.

## Por lado y múltiples valores

Como el [[03 Padding | padding]], acepta de 1 a 4 valores en sentido horario, o propiedades por lado:

```css
border-width: 1px 2px;           /* vertical · horizontal */
border-width: 1px 2px 3px 4px;   /* T R B L */
border-bottom-width: 3px;        /* solo abajo */
```

## px y la nitidez

> [!tip] Los bordes finos van en px
> A diferencia de la tipografía, los bordes finos se expresan mejor en **`px`**: un borde debe ser **nítido y fijo** (`1px`), no escalar con la fuente. Un borde en `rem` puede renderizarse borroso o de grosor inconsistente. Es uno de los pocos casos donde `px` es la elección correcta (ver [[01 Unidades Absolutas (px, pt, cm) | unidades absolutas]]).

## El problema del medio píxel

> [!warning] Bordes "hairline" en pantallas retina
> Conseguir un borde de **exactamente 1 píxel físico** en pantallas de alta densidad (retina) es complicado: `1px` CSS son 2 o 3 píxeles físicos. Técnicas como `transform: scaleY(0.5)` o usar `box-shadow` se emplean para bordes ultrafinos, pero para la mayoría de casos `1px` es suficiente y consistente.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `px` para el grosor del borde (nitidez y consistencia).
> - Evita las palabras `thin`/`medium`/`thick` si necesitas precisión.
> - Recuerda que el grosor **suma** al tamaño de la caja (salvo con `border-box`).
> - Acompaña siempre de un `border-style` o el borde no se verá.

## Errores comunes

> [!warning] Trampas
> - **Grosor sin estilo**: el borde no aparece.
> - **Borde en `rem`**: posible renderizado borroso.
> - **Olvidar que ocupa espacio**: añadir un borde "mueve" el layout si no se compensa.

## Notas relacionadas

- [[02 border-style]] — imprescindible para que el borde se vea.
- [[04 Shorthand border]] — definir grosor, estilo y color de una vez.
- [[01 Unidades Absolutas (px, pt, cm)]] — por qué `px` para bordes.
