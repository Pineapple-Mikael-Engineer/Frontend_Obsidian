---
title: gap — Espacio entre items
aliases:
  - gap
  - flex gap
tags:
  - css
  - api/propiedad
  - layout
propiedad: gap
grupo: flexbox
valor_inicial: "0"
hereda: false
animable: true
draft: false
order: 7
---

# gap

> [!definicion]
> `gap` define el **espacio entre los items** de un contenedor flex (o grid), sin añadir espacio en los bordes exteriores. Es la forma moderna y limpia de separar elementos, que reemplaza los antiguos hacks de márgenes.

```css
.barra { display: flex; gap: 1rem; }
.rejilla { display: grid; gap: 1rem 2rem; }   /* fila columna */
```

## La sintaxis

| Forma | Significa |
|-------|-----------|
| `gap: 1rem` | El mismo espacio entre filas y columnas |
| `gap: 1rem 2rem` | `row-gap` (entre filas) · `column-gap` (entre columnas) |
| `row-gap` / `column-gap` | Las propiedades individuales |

## Solo entre items, no en los bordes

> [!info] gap no añade margen exterior
> La gran virtud de `gap`: el espacio va **solo entre** los items, **no** en los bordes del contenedor. Esto evita el clásico problema del margen ("el último item tiene un margen sobrante que descuadra"):
> ```
> gap:  [A]__[B]__[C]      (sin espacio antes de A ni después de C)
> ```
> Por eso ya no hace falta el `:last-child { margin: 0 }` ni el `margin-left` negativo en el contenedor.

## gap reemplaza los hacks de margin

> [!tip] Prefiere gap a los márgenes para separar
> Antes de `gap` en Flexbox, separar items se hacía con márgenes y trucos para quitar el sobrante:
> ```css
> /* ❌ El método antiguo, frágil */
> .item { margin-right: 1rem; }
> .item:last-child { margin-right: 0; }
>
> /* ✅ El método moderno */
> .contenedor { display: flex; gap: 1rem; }
> ```
> `gap` es más limpio, no deja sobrantes y funciona igual con `flex-wrap` (separa también entre filas). Es una de las mejores incorporaciones recientes de CSS.

## gap con flex-wrap

Con [[03 Ajuste de Línea (flex-wrap) | `flex-wrap: wrap`]], `gap` separa **tanto** entre items de una fila **como** entre las filas, dando una rejilla con espaciado uniforme sin esfuerzo:

```css
.galeria {
  display: flex; flex-wrap: wrap;
  gap: 1rem;   /* separa horizontal y verticalmente */
}
```

## Soporte

> [!info] gap en Flexbox: soporte moderno
> `gap` lleva tiempo en Grid, pero su soporte en **Flexbox** llegó algo más tarde (todos los navegadores modernos lo tienen desde ~2021). En proyectos que deban soportar navegadores muy antiguos, el respaldo es el método de márgenes; en la web actual, `gap` es seguro.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `gap` para separar items en Flexbox y Grid; olvida los hacks de márgenes.
> - `gap: <fila> <columna>` para espaciado distinto en cada eje.
> - Define el espaciado en `rem` (o variables) para coherencia con la escala del diseño.
> - Combínalo con `flex-wrap` para rejillas con espaciado uniforme.

## Errores comunes

> [!warning] Trampas
> - **Seguir usando márgenes** para separar cuando `gap` es más limpio.
> - **Esperar `gap` en los bordes**: solo va entre items.
> - **Confiar en `gap` flex** en navegadores antiguos sin respaldo.

## Notas relacionadas

- [[01 Contenedor Flex (display flex)]] — el contenedor donde se aplica.
- [[07 Margin Collapse]] — el problema de los márgenes que `gap` evita.
- [[04 CSS Grid/index]] — `gap` también es clave en Grid.
