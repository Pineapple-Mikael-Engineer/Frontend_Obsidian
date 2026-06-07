---
title: Imágenes Responsivas — Servir la imagen adecuada a cada pantalla
aliases:
  - responsive images css
tags:
  - css
  - api/concepto
  - responsive
draft: false
---

# Imágenes Responsivas

> [!definicion]
> Las imágenes responsivas adaptan el **tamaño** y, a veces, el **archivo** de una imagen a cada pantalla, para que no se deformen, no desborden y no pesen de más. La parte de HTML (`srcset`, `<picture>`) se trata en el curso de HTML; aquí se cubre la parte de **CSS**: cómo se dimensionan y encajan.

```css
img { max-width: 100%; height: auto; }   /* la regla de oro */
```

## La regla de oro de las imágenes fluidas

> [!tip] max-width: 100% + height: auto
> La regla que evita el 90% de los problemas con imágenes:
> ```css
> img {
>   max-width: 100%;   /* nunca más ancha que su contenedor (no desborda) */
>   height: auto;      /* mantiene la proporción al escalar */
> }
> ```
> `max-width: 100%` impide que una imagen grande se salga de su caja; `height: auto` evita que se deforme al reducir el ancho. Es una de las primeras reglas de cualquier reset moderno.

## object-fit: llenar una caja sin deformar

> [!tip] object-fit para imágenes con tamaño fijo
> Cuando una imagen debe llenar una caja de tamaño/proporción fija (una miniatura, un avatar, una tarjeta), `object-fit` controla **cómo encaja**, igual que `background-size`:
> ```css
> .miniatura {
>   width: 100%;
>   aspect-ratio: 1;
>   object-fit: cover;   /* llena la caja recortando, sin deformar */
> }
> ```
> | `object-fit` | Efecto |
> |--------------|--------|
> | `cover` | Llena la caja, recorta lo que sobra (más usado) |
> | `contain` | Cabe entera, puede dejar huecos |
> | `fill` | Estira para llenar (deforma) — por defecto |
> | `none` | Tamaño original |
>
> `object-position` (como `background-position`) elige qué parte se ve al recortar con `cover`.

## object-fit en img vs. background

> [!info] Cuándo img + object-fit y cuándo background
> `object-fit` permite que un `<img>` (semántico, accesible, con `loading="lazy"`) se comporte como un fondo `cover`. Por eso hoy se prefiere un `<img>` con `object-fit: cover` a una `background-image` para imágenes de contenido: combina la accesibilidad del `<img>` con el control de encaje del fondo.

## Evitar el salto de layout (CLS)

> [!warning] Reserva el espacio de la imagen
> Una imagen sin dimensiones provoca un **salto** cuando carga (el contenido se desplaza). Para reservarlo:
> - Dar `width`/`height` (o `aspect-ratio`) que fije la proporción antes de cargar.
> - El navegador, con las dimensiones en el HTML, reserva el hueco.
> ```css
> img { max-width: 100%; height: auto; aspect-ratio: 16 / 9; }
> ```
> Esto mejora el **CLS** (Cumulative Layout Shift), una métrica de rendimiento.

## La parte de HTML: srcset y picture

Para servir **distinta resolución o formato** según la pantalla (no solo escalar), se usan `srcset`/`sizes` y `<picture>` en el HTML, que sirven archivos más ligeros a móviles. Eso pertenece al curso de HTML ([[02 Imágenes Responsivas (picture, source) | picture/srcset]]); el CSS se encarga del encaje visual.

## Buenas prácticas

> [!tip] Recomendaciones
> - `img { max-width: 100%; height: auto; }` como regla base.
> - `object-fit: cover` para imágenes que llenan cajas de proporción fija.
> - Prefiere `<img>` + `object-fit` a `background-image` para imágenes de contenido (accesible + lazy).
> - Reserva el espacio con `aspect-ratio` o dimensiones para evitar saltos (CLS).

## Errores comunes

> [!warning] Trampas
> - **Sin `max-width: 100%`**: las imágenes desbordan en móvil.
> - **`object-fit: fill`** (por defecto) que deforma la imagen.
> - **Sin dimensiones**: salto de layout al cargar.

## Notas relacionadas

- [[02 Imágenes Responsivas (picture, source)]] — `srcset`/`<picture>` desde HTML.
- [[03 aspect-ratio]] — reservar la proporción de la imagen.
- [[05 background-size]] — `cover`/`contain` en fondos (paralelo a object-fit).
