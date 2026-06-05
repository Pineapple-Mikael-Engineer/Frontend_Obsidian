---
title: border-radius — Esquinas redondeadas
aliases:
  - border-radius
tags:
  - css
  - api/propiedad
  - layout
propiedad: border-radius
grupo: box-model
valor_inicial: "0"
hereda: false
animable: true
draft: false
---

# border-radius

> [!definicion]
> `border-radius` **redondea las esquinas** de una caja. Pese al nombre, no requiere un borde visible: redondea también el fondo y el recorte del contenido. Es una de las propiedades más usadas para suavizar el aspecto de tarjetas, botones y avatares.

```css
.tarjeta { border-radius: 0.5rem; }
.avatar  { border-radius: 50%; }    /* círculo (si es cuadrado) */
.pildora { border-radius: 9999px; } /* extremos totalmente redondeados */
```

## Valores y formas

| Valor | Efecto |
|-------|--------|
| Una longitud | Mismo radio en las 4 esquinas (`8px`, `0.5rem`) |
| Porcentaje | Relativo al tamaño de la caja (`50%` = círculo/elipse) |
| 4 valores | Una por esquina, desde arriba-izquierda en horario |
| `r1 / r2` | Radio horizontal `/` vertical (esquinas elípticas) |

```css
border-radius: 1rem 0 1rem 0;   /* esquinas alternas redondeadas */
border-radius: 2rem / 1rem;     /* elípticas: 2rem horizontal, 1rem vertical */
```

## Los dos patrones estrella

> [!tip] Círculo y píldora
> Dos recetas que conviene memorizar:
> - **Círculo**: `border-radius: 50%` sobre un elemento **cuadrado** (mismo ancho y alto) lo convierte en círculo perfecto. Sobre un rectángulo, da una elipse.
> - **Píldora** (extremos semicirculares): un radio enorme (`9999px` o `100vmax`) redondea los lados cortos por completo, sin importar el tamaño:
> ```css
> .badge { border-radius: 9999px; }   /* botón "pill" */
> ```

## Esquinas individuales

Cada esquina tiene su propiedad, útil para formas asimétricas:

```css
border-top-left-radius: 1rem;
border-bottom-right-radius: 1rem;
/* lógicas: border-start-start-radius, etc. */
```

## Cómo recorta el contenido

> [!info] El radius recorta el fondo, no siempre el contenido desbordado
> `border-radius` redondea el fondo y el borde, pero el contenido que **desborda** (una imagen hija que llena la caja) puede sobresalir por las esquinas cuadradas. Para que el contenido también se recorte con las esquinas redondeadas, se añade `overflow: hidden`:
> ```css
> .tarjeta-img {
>   border-radius: 0.5rem;
>   overflow: hidden;   /* la imagen interior también se redondea */
> }
> ```

## El radio se reduce si no cabe

Si la suma de radios de un lado supera el tamaño de la caja, el navegador los **reduce proporcionalmente** para que quepan. Por eso un `border-radius: 9999px` no rompe nada: se ajusta al máximo posible (media altura), dando la píldora.

## Buenas prácticas

> [!tip] Recomendaciones
> - `50%` sobre un cuadrado para círculos; un radio enorme para píldoras.
> - Añade `overflow: hidden` para recortar el contenido (imágenes) con las esquinas.
> - Usa `rem` para que el redondeo escale de forma coherente con el diseño.
> - Define un radio base en variables (`--radio: 0.5rem`) para consistencia.

## Errores comunes

> [!warning] Trampas
> - **Imagen que sobresale** de las esquinas: falta `overflow: hidden`.
> - **`50%` en un rectángulo** esperando un círculo: da una elipse.
> - **Radio en `%`** sin recordar que es relativo al tamaño de la caja.

## Notas relacionadas

- [[04 Shorthand border]] — el borde que `border-radius` redondea.
- [[01 clip-path]] — recortes con formas más complejas que el redondeo.
- [[05 background-size]] — el fondo que el radius también redondea.
