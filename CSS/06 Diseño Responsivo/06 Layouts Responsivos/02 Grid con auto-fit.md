---
title: Grid con auto-fit — La rejilla responsiva por excelencia
aliases:
  - grid auto-fit responsive
  - RAM pattern
tags:
  - css
  - api/concepto
  - responsive
draft: false
---

# Grid con auto-fit

> [!definicion]
> El patrón **Grid con `auto-fit`** crea una galería que **ajusta el número de columnas** al ancho disponible, sin media queries: el navegador mete tantas columnas como quepan de un tamaño mínimo, y reparte el espacio sobrante. Es la receta de rejilla responsiva más usada del CSS moderno.

```css
.galeria {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(15rem, 1fr));
  gap: 1rem;
}
```

## La línea mágica, explicada

> [!tip] repeat(auto-fit, minmax(<min>, 1fr))
> Esta única declaración hace toda la magia responsiva:
> ```css
> grid-template-columns: repeat(auto-fit, minmax(15rem, 1fr));
> ```
> - **`minmax(15rem, 1fr)`**: cada columna mide **al menos 15rem**; si sobra espacio, crece (`1fr`).
> - **`auto-fit`**: crea **tantas columnas como quepan** de ese tamaño.
>
> Pantalla ancha → varias columnas. Pantalla estrecha → menos columnas, hasta una sola en móvil. **Sin una sola media query.** Por eso a veces se la llama "el patrón RAM" (Repeat, Auto, Minmax).

## auto-fit vs. auto-fill

> [!info] La diferencia con pocos items
> Con **menos items que columnas posibles**:
> - **`auto-fit`**: las columnas vacías colapsan y los items **se estiran** para llenar el ancho.
> - **`auto-fill`**: las columnas vacías se mantienen; los items **no se estiran** (quedan a 15rem con huecos a la derecha).
>
> **`auto-fit`** es lo deseado casi siempre (los items llenan el ancho). Detalle en [[12 Patrones (auto-fit, auto-fill)]].

## Evitar el desbordamiento en móvil

> [!warning] El mínimo puede desbordar pantallas estrechas
> Si `15rem` es **más ancho que la pantalla**, la columna no puede encoger y **desborda** (scroll horizontal en móviles pequeños). La versión robusta usa `min()`:
> ```css
> grid-template-columns: repeat(auto-fit, minmax(min(15rem, 100%), 1fr));
> ```
> `min(15rem, 100%)` usa 15rem salvo que el contenedor sea más estrecho, evitando el scroll horizontal. **Es la versión que conviene memorizar.**

## La receta completa

```css
.galeria {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(16rem, 100%), 1fr));
  gap: clamp(1rem, 3vw, 2rem);   /* gap también fluido */
}
```
Galería que va de 1 a N columnas según el ancho, con un gap que también escala, sin media queries y sin desbordar en móvil.

## Por qué supera al flex-wrap

> [!tip] Columnas alineadas en la última fila
> Frente a [[01 Flex-wrap para Componentes | flex-wrap]], Grid `auto-fit` tiene una ventaja: las columnas están **alineadas** en una rejilla real, así que la **última fila** mantiene el ancho de columna (no se estiran items para llenar). Para galerías de tarjetas donde la alineación importa, Grid es superior.

## Buenas prácticas

> [!tip] Recomendaciones
> - Memoriza `repeat(auto-fit, minmax(min(<min>, 100%), 1fr))` para galerías responsivas.
> - Usa `auto-fit` (items se estiran) salvo que quieras columnas de ancho fijo (`auto-fill`).
> - Combina con `gap: clamp(...)` para un espaciado también fluido.
> - Para columnas alineadas, Grid es mejor que flex-wrap.

## Errores comunes

> [!warning] Trampas
> - **Desbordamiento en móvil** sin `min(<min>, 100%)`.
> - **Confundir `auto-fit` con `auto-fill`**: resultados distintos con pocos items.
> - **Olvidar `minmax()`**: `repeat(auto-fit, 1fr)` no funciona como rejilla responsiva.

## Notas relacionadas

- [[12 Patrones (auto-fit, auto-fill)]] — la explicación completa de `auto-fit`/`auto-fill`.
- [[05 minmax() y fit-content()]] — el `minmax()` del patrón.
- [[01 Flex-wrap para Componentes]] — la alternativa con Flexbox.
