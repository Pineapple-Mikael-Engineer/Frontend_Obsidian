---
title: minmax() y fit-content() — Pistas con límites
aliases:
  - minmax
  - fit-content grid
tags:
  - css
  - api/funcion
  - layout
draft: false
---

# minmax() y fit-content()

> [!definicion]
> `minmax(min, max)` define una pista de Grid que **no baja de un mínimo ni sube de un máximo**, adaptándose entre ambos. `fit-content(límite)` dimensiona la pista según su contenido, hasta un tope. Ambas dan a las pistas un comportamiento flexible pero acotado.

```css
grid-template-columns: minmax(200px, 1fr) 3fr;
grid-template-columns: fit-content(300px) 1fr;
```

## minmax(): el rango de una pista

> [!info] Mínimo y máximo en una pista
> `minmax(min, max)` hace que la pista mida entre `min` y `max` según el espacio:
> ```css
> grid-template-columns: minmax(200px, 1fr);
> /* nunca menos de 200px, pero crece como 1fr si hay sitio */
> ```
> Es clave para rejillas responsivas: una columna que se encoge hasta cierto punto pero no más (evita que el contenido se aplaste), y crece para repartir el sobrante.

## La receta responsiva con minmax

> [!tip] minmax + auto-fit = galería responsiva
> El patrón más usado de Grid responsivo:
> ```css
> grid-template-columns: repeat(auto-fit, minmax(15rem, 1fr));
> ```
> - `minmax(15rem, 1fr)`: cada columna mide **al menos 15rem**, y crece (`1fr`) para llenar.
> - `auto-fit`: mete tantas columnas como quepan.
>
> Resultado: una galería que pasa de 1 a 2 a 3+ columnas según el ancho, sin una sola media query. La columna nunca baja de 15rem (no se aplasta) y el espacio sobrante se reparte. Detalle en [[12 Patrones (auto-fit, auto-fill)]].

## El problema de minmax(15rem, 1fr) en pantallas estrechas

> [!warning] El mínimo puede desbordar en móvil
> Si el `min` de `minmax` (p. ej. `15rem`) es **más ancho que la pantalla**, la columna no puede encogerse por debajo y **desborda** (scroll horizontal en móvil). La solución es usar `min()` para que el mínimo ceda en pantallas estrechas:
> ```css
> grid-template-columns: repeat(auto-fit, minmax(min(15rem, 100%), 1fr));
> ```
> `min(15rem, 100%)` usa 15rem salvo que el contenedor sea más estrecho, evitando el desbordamiento.

## fit-content(): contenido con tope

`fit-content(<límite>)` dimensiona la pista según su **contenido**, pero sin pasar del límite. Útil para una columna que se ajusta a lo que contiene (un menú, una etiqueta) pero no se dispara:

```css
/* La primera columna se ajusta al contenido, máximo 200px; la segunda toma el resto */
grid-template-columns: fit-content(200px) 1fr;
```

## Buenas prácticas

> [!tip] Recomendaciones
> - `minmax(<min>, 1fr)` para columnas que no se aplastan pero crecen.
> - `repeat(auto-fit, minmax(<min>, 1fr))` para galerías responsivas.
> - Usa `minmax(min(<min>, 100%), 1fr)` para evitar el desbordamiento en móvil.
> - `fit-content(<tope>)` para columnas que se ajustan al contenido con un máximo.

## Errores comunes

> [!warning] Trampas
> - **`minmax(15rem, 1fr)` que desborda** en pantallas más estrechas que el mínimo.
> - **Olvidar `minmax`** con `auto-fit` (las columnas no tendrían tamaño definido).
> - **Confundir `fit-content()` función** con la palabra clave `fit-content`.

## Notas relacionadas

- [[04 Unidad fr y repeat()]] — `fr` y `repeat()`, que acompañan a `minmax()`.
- [[12 Patrones (auto-fit, auto-fill)]] — la receta responsiva completa.
- [[04 Tamaños de Contenido (fit-content, min-content, max-content)]] — los tamaños intrínsecos.
