---
title: conic-gradient — Gradiente cónico
aliases:
  - conic-gradient
tags:
  - css
  - api/funcion
  - fondos
draft: false
---

# conic-gradient

> [!definicion]
> `conic-gradient()` genera una transición de color que **gira alrededor de un centro**, como las manecillas de un reloj: los colores cambian según el **ángulo**, no según la distancia al centro. Es la base de gráficos circulares (pie charts), ruedas de color, indicadores de progreso y efectos giratorios.

```css
background: conic-gradient(#cba6f7, #f38ba8, #89dceb, #cba6f7);
```

## Sintaxis

```css
conic-gradient(from <ángulo> at <posición>, <color1> <ángulo1>, ...)
```

| Parte | Ejemplo | Significa |
|-------|---------|-----------|
| `from <ángulo>` | `from 90deg` | Desde qué ángulo empieza (por defecto, arriba) |
| `at <posición>` | `at center` | El centro de giro |
| Color stops | `red 0deg, blue 120deg` | Colores y su ángulo |

> [!info] Los stops van en grados (o %), no en distancia
> A diferencia de los gradientes lineal y radial (donde los stops son posiciones a lo largo de una distancia), en `conic-gradient` los stops son **ángulos** (`0deg`–`360deg`) o porcentajes del giro. El color cambia al **rotar**, manteniéndose constante hacia fuera desde el centro.

## La receta estrella: gráfico circular (pie chart)

> [!tip] Un pie chart con transiciones duras
> Usando stops en el mismo ángulo (transición dura), `conic-gradient` dibuja un gráfico de sectores **sin SVG ni JS**:
> ```css
> .pie {
>   width: 150px; aspect-ratio: 1; border-radius: 50%;
>   background: conic-gradient(
>     #cba6f7 0deg 120deg,    /* sector 1: 33% */
>     #89dceb 120deg 200deg,  /* sector 2 */
>     #f38ba8 200deg 360deg   /* sector 3 */
>   );
> }
> ```
> Cada sector va de un ángulo a otro; `border-radius: 50%` lo hace circular.

## Más recetas

```css
/* Rueda de color (matiz completo) */
.rueda {
  border-radius: 50%;
  background: conic-gradient(in oklch, red, yellow, lime, cyan, blue, magenta, red);
}

/* Anillo de progreso (con máscara o radial encima) */
.progreso {
  background: conic-gradient(#cba6f7 var(--pct, 25%), #313244 0);
  border-radius: 50%;
}

/* Patrón de "tablero" o efecto giratorio animado */
@keyframes girar { to { --a: 360deg; } }
```

## Indicadores de progreso

> [!tip] Anillo de progreso con una variable
> Combinando `conic-gradient` con una [[01 Definición y Uso (--var, var()) | variable]] que controla el ángulo, se hace un indicador de progreso circular que se actualiza con JS o una animación:
> ```css
> .anillo {
>   background: conic-gradient(#cba6f7 calc(var(--p) * 1%), #313244 0);
> }
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsalo para gráficos circulares, ruedas de color e indicadores de progreso sin SVG.
> - Stops en el mismo ángulo para sectores netos (pie chart).
> - `from <ángulo>` para rotar el punto de inicio; `at` para mover el centro.
> - Interpola `in oklch` para ruedas de color vivas.

## Errores comunes

> [!warning] Trampas
> - **Olvidar `border-radius: 50%`** al hacer un pie chart (sale cuadrado).
> - **Confundir ángulos con distancias**: los stops son grados, no posiciones radiales.
> - **No cerrar el ciclo** (repetir el primer color al final) en ruedas: deja una "costura".

## Notas relacionadas

- [[01 linear-gradient]] · [[02 radial-gradient]] — los otros tipos.
- [[01 Definición y Uso (--var, var())]] — variables para progreso dinámico.
- [[04 Gradientes Repetidos]] — gradientes cónicos repetidos (estrellas, rayos).
