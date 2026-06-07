---
title: flex-grow, flex-shrink, flex-basis — Cómo crecen y encogen los items
aliases:
  - flex-grow
  - flex-shrink
  - flex-basis
  - flex shorthand
tags:
  - css
  - api/propiedad
  - layout
propiedad: flex
grupo: flexbox
valor_inicial: "0 1 auto"
hereda: false
animable: true
draft: false
---

# flex-grow, flex-shrink, flex-basis

> [!definicion]
> Estas tres propiedades de los **items** controlan cómo reparten el espacio del contenedor: `flex-grow` (cuánto **crecen** para llenar el sobrante), `flex-shrink` (cuánto **encogen** si falta espacio) y `flex-basis` (su tamaño **base** de partida). El shorthand `flex` las combina.

```css
.item { flex: 1; }          /* crece para repartir el espacio por igual */
.item { flex: 1 1 15rem; }  /* grow shrink basis */
```

## El shorthand flex

```css
flex: <grow> <shrink> <basis>;
```

| Valor | grow | shrink | basis |
|-------|------|--------|-------|
| `flex: 1` | 1 | 1 | 0 |
| `flex: auto` | 1 | 1 | auto |
| `flex: none` | 0 | 0 | auto |
| `flex: 1 1 15rem` | 1 | 1 | 15rem |

> [!info] flex: 1 es el más usado
> `flex: 1` (= `1 1 0`) hace que el item **crezca para repartir el espacio sobrante por igual** con sus hermanos que también tengan `flex: 1`. Es la receta para columnas de igual ancho:
> ```css
> .columnas > * { flex: 1; }   /* todas las columnas iguales */
> ```

## flex-grow: repartir el sobrante

`flex-grow` es un **factor** de reparto del espacio sobrante. Con `flex-grow: 1` en todos, se reparte por igual; con valores distintos, proporcionalmente:

```css
.a { flex-grow: 2; }   /* recibe el doble del espacio sobrante que .b */
.b { flex-grow: 1; }
```

`flex-grow: 0` (por defecto) significa que el item **no crece** (se queda en su `flex-basis`).

## flex-shrink: encoger cuando falta espacio

`flex-shrink` controla cuánto encoge un item si los items no caben. Por defecto es `1` (todos encogen). `flex-shrink: 0` impide que un item encoja, útil para que un icono o botón mantenga su tamaño:

```css
.icono { flex-shrink: 0; }   /* no se aplasta aunque falte espacio */
```

## flex-basis: el tamaño de partida

`flex-basis` es el tamaño **inicial** del item en el eje principal, **antes** de crecer o encoger. Es como un `width` (en row) pero específico de flex:

| `flex-basis` | Significa |
|--------------|-----------|
| `auto` | El tamaño del contenido (o el `width` si lo hay) |
| `0` | Ignora el contenido; el reparto es puro `flex-grow` |
| `15rem` | Tamaño ideal de 15rem (antes de ajustar) |

> [!info] flex: 1 (basis 0) vs. flex: auto (basis auto)
> La diferencia sutil pero importante:
> - **`flex: 1`** (basis `0`): los items se reparten el espacio **por igual**, ignorando su contenido. Columnas idénticas.
> - **`flex: auto`** (basis `auto`): el reparto parte del **tamaño del contenido**, así que un item con más texto queda más ancho. Columnas proporcionales al contenido.

## La rejilla fluida

> [!tip] flex: 1 1 <ancho> + wrap = rejilla responsiva
> Combinando los tres con [[03 Ajuste de Línea (flex-wrap) | wrap]], se hace una rejilla que se reorganiza sola:
> ```css
> .galeria { display: flex; flex-wrap: wrap; gap: 1rem; }
> .galeria > * { flex: 1 1 15rem; }   /* ideal 15rem, crece y encoge, salta de línea */
> ```

## El min-width: auto que desborda

> [!warning] Un item flex no encoge por debajo de su contenido
> Por defecto, un item flex tiene `min-width: auto`, lo que le impide encoger **por debajo del tamaño de su contenido** (una palabra larga, una imagen). Esto causa el clásico item que **desborda** el contenedor flex. La solución: `min-width: 0` en el item:
> ```css
> .item { flex: 1; min-width: 0; }   /* permite encoger por debajo del contenido */
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - `flex: 1` para columnas de igual ancho; `flex: 1 1 <ancho>` para rejillas fluidas.
> - `flex-shrink: 0` en iconos/botones que no deben aplastarse.
> - `min-width: 0` en items que desbordan por su contenido.
> - Entiende `basis: 0` (reparto igual) vs. `basis: auto` (proporcional al contenido).

## Errores comunes

> [!warning] Trampas
> - **Item que desborda** el contenedor: pon `min-width: 0`.
> - **Confundir `flex: 1` con `flex: auto`**: uno reparte igual, otro según contenido.
> - **Olvidar `flex-shrink: 0`** en elementos que se aplastan sin querer.

## Notas relacionadas

- [[01 Contenedor Flex (display flex)]] — el contenedor que reparte el espacio.
- [[03 Ajuste de Línea (flex-wrap)]] — el wrap de las rejillas fluidas.
- [[02 min y max (width, height)]] — el `min-width: auto` que desborda.
