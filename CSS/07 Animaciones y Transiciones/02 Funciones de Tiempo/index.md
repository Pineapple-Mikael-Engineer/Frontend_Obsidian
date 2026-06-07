---
title: Funciones de Tiempo — Las curvas de aceleración (easing)
aliases:
  - timing functions
  - easing functions
tags:
  - css
  - api/concepto
  - animacion
draft: false
---

# Funciones de Tiempo

> [!definicion]
> Una **función de tiempo** (easing) define **cómo se distribuye** una transición o animación en el tiempo: a velocidad constante (`linear`), acelerando/frenando (las curvas `ease`), en saltos (`steps()`) o con una curva totalmente a medida (`cubic-bezier()`). Es lo que da carácter al movimiento.

```css
transition-timing-function: ease-out;
animation-timing-function: cubic-bezier(0.34, 1.56, 0.64, 1);   /* con rebote */
```

## Mapa de la subsección

- [[01 Curvas (ease, linear, ease-in-out)]] — las curvas predefinidas y cuándo usarlas.
- [[02 steps()]] — animación en saltos discretos (sprites, máquinas de escribir).
- [[03 cubic-bezier()]] — curvas a medida con control total.

## Las dos familias

> [!info] Curvas suaves vs. saltos
> Las funciones de tiempo se dividen en:
> - **Curvas continuas** (`ease`, `cubic-bezier`): el movimiento es **suave**, interpolando todos los valores intermedios. Para casi todo.
> - **Saltos** (`steps()`): el movimiento avanza en **pasos discretos**, sin valores intermedios. Para sprites animados, efectos de "máquina de escribir", relojes.

## El easing define la personalidad

> [!tip] La curva comunica intención
> La misma animación (misma duración y distancia) se **siente** muy distinta según el easing:
> - `linear`: mecánico, robótico.
> - `ease-out`: reactivo, "llega y se asienta".
> - `cubic-bezier` con rebote: juguetón, enérgico.
>
> Un sistema de diseño suele definir **dos o tres** curvas propias (una estándar, una de entrada, una con carácter) y reutilizarlas en variables para una sensación coherente:
> ```css
> :root {
>   --ease-estandar: cubic-bezier(0.4, 0, 0.2, 1);
>   --ease-entrada: cubic-bezier(0, 0, 0.2, 1);
>   --ease-rebote: cubic-bezier(0.34, 1.56, 0.64, 1);
> }
> ```

## Notas relacionadas

- [[01 Curvas (ease, linear, ease-in-out)]] — el punto de partida.
- [[03 cubic-bezier()]] — definir curvas propias.
- [[03 transition-timing-function]] — donde se aplican en transiciones.
