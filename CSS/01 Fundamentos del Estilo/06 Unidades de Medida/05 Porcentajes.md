---
title: Porcentajes — Relativo al contenedor
aliases:
  - percentages
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 5
---

# Porcentajes

> [!definicion]
> Un **porcentaje** (`%`) expresa una longitud relativa a una **referencia** que depende de la propiedad: casi siempre una dimensión del elemento **padre**. `width: 50%` es la mitad del ancho del contenedor. Es la unidad clave del layout fluido clásico.

```css
.columna { width: 50%; }    /* la mitad del ancho del padre */
.media   { width: 80%; max-width: 60rem; }   /* fluido pero acotado */
```

## La referencia cambia según la propiedad

> [!warning] El % no siempre se refiere a lo que parece
> Lo más confuso de los porcentajes: la referencia **depende de la propiedad**:
> | Propiedad con `%` | Se refiere a… |
> |-------------------|---------------|
> | `width`, `left`, `margin`, `padding` | El **ancho** del bloque contenedor |
> | `height`, `top` | El **alto** del bloque contenedor |
> | `font-size` | El `font-size` del **padre** |
> | `line-height` | El propio `font-size` del elemento |
> | `transform: translate()` | El tamaño del **propio** elemento |
>
> El caso más sorprendente: **`padding` y `margin` verticales** también se calculan sobre el **ancho** del contenedor, no el alto. Por eso `padding-top: 50%` crea un cuadrado proporcional al ancho (truco clásico de aspect-ratio antes de [[03 aspect-ratio | `aspect-ratio`]]).

## El problema de height: %

> [!warning] height: 100% necesita un padre con altura
> Un `height: 50%` solo funciona si el **contenedor tiene una altura definida**. Si el padre tiene altura automática (la que le da su contenido), el porcentaje no tiene referencia y se ignora (el elemento toma altura automática). Por eso `height: 100%` en cadena requiere que **todos** los ancestros tengan altura, hasta `html, body { height: 100% }`. En layout moderno, esto se resuelve mejor con Flexbox/Grid o unidades de viewport, evitando cadenas de `height: 100%`.

## El truco padding-top para proporciones

Antes de `aspect-ratio`, mantener una proporción (16:9) se hacía con `padding-top` en porcentaje (calculado sobre el ancho):

```css
.video-16-9 {
  width: 100%;
  padding-top: 56.25%;   /* 9/16 = 56.25% del ancho */
  position: relative;
}
```

Hoy se prefiere [[03 aspect-ratio | `aspect-ratio: 16 / 9`]], mucho más claro.

## Porcentaje vs. otras unidades de layout

| Para anchos | Considera |
|-------------|-----------|
| Columna proporcional simple | `%` |
| Reparto flexible | Flexbox (`flex`) o Grid (`fr`) |
| Acotar la medida de lectura | `ch` + `max-width` en `rem` |

En layout moderno, **Grid (`fr`)** y **Flexbox** suelen ser más limpios que repartir con `%`, sobre todo con `gap` de por medio (los `%` no descuentan el `gap`).

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `%` para anchos fluidos simples, combinado con `max-width` en `rem`.
> - Recuerda que `padding`/`margin` verticales en `%` se calculan sobre el **ancho**.
> - Para alturas, prefiere Flexbox/Grid o unidades de viewport a cadenas de `height: 100%`.
> - Para proporciones, usa `aspect-ratio`, no el truco de `padding-top`.

## Errores comunes

> [!warning] Trampas
> - **`height: %`** sin un padre con altura definida: se ignora.
> - **Asumir que el `%` vertical de padding va sobre el alto**: va sobre el ancho.
> - **Repartir columnas con `%`** sin contar el `gap`: se desborda.

## Notas relacionadas

- [[03 aspect-ratio]] — la forma moderna de mantener proporciones.
- [[04 Unidad fr y repeat()]] — el reparto flexible en Grid.
- [[01 Contenedor Flex (display flex)]] — repartir espacio sin `%`.
