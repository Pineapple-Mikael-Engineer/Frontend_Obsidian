---
title: animation-timing-function y delay — Curva y retardo
aliases:
  - animation-timing-function
  - animation-delay
tags:
  - css
  - api/propiedad
  - animacion
propiedad: animation-timing-function
grupo: animacion
valor_inicial: ease
hereda: false
animable: false
draft: false
order: 3
---

# animation-timing-function y delay

> [!definicion]
> `animation-timing-function` define la **curva de aceleración** de la animación (igual que en transiciones), y `animation-delay` el **retardo** antes de que empiece. Junto con la duración, controlan el ritmo de la animación.

```css
.x {
  animation: pulso 1.5s ease-in-out 0.3s infinite;
  /*                    curva       delay */
}
```

## animation-timing-function

Acepta las mismas [[02 Funciones de Tiempo/index | funciones de tiempo]] que las transiciones: `ease`, `linear`, `ease-out`, `cubic-bezier()`, `steps()`.

> [!info] La curva aplica a cada tramo entre fotogramas
> Un matiz importante: en una animación de varios fotogramas, la `animation-timing-function` se aplica **entre cada par de fotogramas**, no a toda la animación. `0% → 50% → 100%` con `ease` acelera/frena en **cada** tramo. Para cambiar la curva de un tramo concreto, se pone la `timing-function` **dentro** del fotograma:
> ```css
> @keyframes saltar {
>   0%   { transform: translateY(0); animation-timing-function: ease-out; }
>   50%  { transform: translateY(-30px); animation-timing-function: ease-in; }
>   100% { transform: translateY(0); }
> }
> ```
> Esto da curvas distintas para la subida (ease-out) y la bajada (ease-in), simulando gravedad.

## linear para giros continuos

> [!tip] Los spinners van en linear
> A diferencia de la UI reactiva (donde `ease-out` manda), las animaciones **continuas en bucle** (un spinner, un fondo que gira) usan `linear`: una velocidad constante evita los "tirones" que un `ease` produciría en cada repetición:
> ```css
> .spinner { animation: girar 1s linear infinite; }
> ```

## animation-delay

`animation-delay` retrasa el inicio, útil para **escalonar** animaciones (efecto cascada de entrada):

```css
.item:nth-child(1) { animation-delay: 0s; }
.item:nth-child(2) { animation-delay: 0.1s; }
.item:nth-child(3) { animation-delay: 0.2s; }
```

## El delay negativo: empezar "a mitad"

> [!tip] Delay negativo para desfasar bucles
> Un `animation-delay` **negativo** hace que la animación arranque como si ya llevara ese tiempo en marcha (empieza avanzada). Es muy útil para **desfasar** varias copias de la misma animación en bucle, creando ondas:
> ```css
> .punto { animation: rebotar 1s infinite; }
> .punto:nth-child(2) { animation-delay: -0.8s; }   /* arranca avanzada */
> .punto:nth-child(3) { animation-delay: -0.6s; }
> /* los puntos rebotan desfasados, en onda */
> ```
> A diferencia de un delay positivo (que **espera**), el negativo **adelanta**, así que la animación se ve desde el primer instante pero en un punto distinto de su ciclo.

## Buenas prácticas

> [!tip] Recomendaciones
> - `linear` para animaciones continuas en bucle (spinners); `ease-out` para entradas.
> - Pon la `timing-function` dentro de un fotograma para curvas por tramo (gravedad, etc.).
> - Delays crecientes para cascadas de entrada.
> - **Delays negativos** para desfasar bucles (ondas) sin retrasar el arranque visible.

## Errores comunes

> [!warning] Trampas
> - **`ease` en un bucle continuo**: tirones en cada repetición (usa `linear`).
> - **Confundir delay positivo (espera) con negativo (adelanta)**.
> - **Esperar que la curva aplique a toda la animación**: aplica entre cada par de fotogramas.

## Notas relacionadas

- [[02 Funciones de Tiempo/index]] — las curvas disponibles.
- [[04 animation-iteration-count]] — el bucle (`infinite`).
- [[08 Múltiples Animaciones]] — desfasar varias copias.
