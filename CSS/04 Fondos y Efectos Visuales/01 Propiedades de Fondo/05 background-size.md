---
title: background-size — Tamaño de la imagen de fondo
aliases:
  - background-size
  - cover
  - contain
tags:
  - css
  - api/propiedad
  - fondos
propiedad: background-size
grupo: fondo
valor_inicial: auto
hereda: false
animable: true
draft: false
---

# background-size

> [!definicion]
> `background-size` define a qué **tamaño** se muestra la imagen de fondo. Sus dos palabras clave —`cover` y `contain`— resuelven la inmensa mayoría de los casos: llenar la caja recortando, o caber entera. También acepta dimensiones concretas.

```css
.hero  { background-size: cover; }
.logo-fondo { background-size: contain; }
```

## cover vs. contain: la decisión clave

> [!info] Llenar recortando vs. caber entero
> Las dos palabras resuelven necesidades opuestas:
> - **`cover`**: escala la imagen para **cubrir toda la caja**, recortando lo que sobresale. Mantiene la proporción. La imagen siempre llena el fondo, pero **se corta** por uno de los ejes. Ideal para fotos de fondo (heroes, tarjetas).
> - **`contain`**: escala para que la imagen **quepa entera** dentro de la caja, sin recortar. Mantiene la proporción, pero puede **dejar huecos** (la imagen no llena los dos ejes). Ideal para logos o ilustraciones que deben verse completas.
>
> | | `cover` | `contain` |
> |--|---------|-----------|
> | ¿Llena la caja? | Sí | No necesariamente |
> | ¿Recorta? | Sí | No |
> | ¿Deja huecos? | No | Posiblemente |
> | Uso típico | Fotos de fondo | Logos, ilustraciones completas |

## Otras formas de valor

| Valor | Efecto |
|-------|--------|
| `auto` | Tamaño real de la imagen (por defecto) |
| `100px 50px` | Ancho y alto concretos |
| `50%` | Porcentaje del contenedor (ancho; alto `auto`) |
| `cover` / `contain` | Ver arriba |

```css
background-size: 200px auto;   /* 200px de ancho, alto proporcional */
background-size: 100% 100%;    /* estira a la caja (¡puede deformar!) */
```

> [!warning] 100% 100% deforma la imagen
> `background-size: 100% 100%` estira la imagen para llenar **ambos** ejes ignorando su proporción, **deformándola**. Casi siempre lo que se quiere es `cover` (llena sin deformar, recortando). Reserva `100% 100%` para gradientes o casos donde la deformación no importe.

## Ejemplos de uso

```css
/* Foto de hero que llena sin deformar */
.hero { background: url("foto.jpg") center / cover no-repeat; }

/* Logo de marca de agua, completo y centrado */
.marca-agua {
  background: url("logo.svg") center / contain no-repeat;
  opacity: 0.05;
}

/* Patrón de puntos con cuadrícula de 20px */
.puntos {
  background-image: radial-gradient(#cba6f7 2px, transparent 2px);
  background-size: 20px 20px;
}
```

## El tamaño de los patrones generados

Con gradientes o patrones, `background-size` define el **tamaño de la celda** que se repite, controlando la densidad del patrón (un punto cada 20px, líneas cada 10px). Es clave para texturas CSS.

## Buenas prácticas

> [!tip] Recomendaciones
> - `cover` para fotos de fondo que deben llenar (el caso más común).
> - `contain` para logos/ilustraciones que deben verse enteros.
> - Evita `100% 100%` salvo gradientes (deforma fotos).
> - Con patrones, usa `background-size` para fijar la densidad de la cuadrícula.
> - Combina `cover` con [[04 background-position | `position`]] para elegir el recorte visible.

## Errores comunes

> [!warning] Trampas
> - **`100% 100%`** en fotos: las deforma; usa `cover`.
> - **Esperar que `contain` llene** la caja: puede dejar huecos.
> - **Recorte mal centrado** con `cover`: ajusta `background-position`.

## Notas relacionadas

- [[04 background-position]] — qué parte se ve al recortar con `cover`.
- [[03 aspect-ratio]] — controlar la proporción de la caja del fondo.
- [[08 Shorthand background]] — `size` en el shorthand (`center / cover`).
