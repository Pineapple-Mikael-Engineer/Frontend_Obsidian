---
title: cubic-bezier() — Curvas de aceleración a medida
aliases:
  - cubic-bezier
tags:
  - css
  - api/funcion
  - animacion
draft: false
---

# cubic-bezier()

> [!definicion]
> `cubic-bezier(x1, y1, x2, y2)` define una **curva de aceleración a medida** mediante dos puntos de control, dando control total sobre cómo se distribuye la animación en el tiempo. Permite crear curvas con **rebote**, anticipación o cualquier sensación que las predefinidas no cubren.

```css
transition-timing-function: cubic-bezier(0.34, 1.56, 0.64, 1);   /* con overshoot */
```

## Cómo funciona

> [!info] Dos puntos de control sobre una curva
> La curva va siempre de (0,0) a (1,1) — del inicio al final de la animación. Los **cuatro números** son las coordenadas de **dos puntos de control** que "tiran" de la curva:
> - `x1, y1`: el primer punto de control (afecta al arranque).
> - `x2, y2`: el segundo (afecta al final).
>
> El eje X es el **tiempo** (0 a 1) y el eje Y el **progreso** de la animación. Una curva más empinada = movimiento más rápido en ese tramo.

## Las predefinidas son cubic-bezier

Las curvas con nombre equivalen a `cubic-bezier` concretas:

| Curva | Equivale a |
|-------|------------|
| `ease` | `cubic-bezier(0.25, 0.1, 0.25, 1)` |
| `linear` | `cubic-bezier(0, 0, 1, 1)` |
| `ease-in` | `cubic-bezier(0.42, 0, 1, 1)` |
| `ease-out` | `cubic-bezier(0, 0, 0.58, 1)` |
| `ease-in-out` | `cubic-bezier(0.42, 0, 0.58, 1)` |

## El efecto rebote (overshoot)

> [!tip] Valores de y fuera de [0,1] para rebote
> Lo más potente de `cubic-bezier`: si `y1` o `y2` salen del rango **[0, 1]**, la curva **sobrepasa** y vuelve, creando un efecto de **rebote** o "overshoot" (el elemento se pasa de su destino y regresa), muy usado para sensaciones juguetonas:
> ```css
> /* El elemento se pasa de su tamaño y vuelve (rebote) */
> transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
> ```
> El `1.56` (mayor que 1) hace que la animación sobrepase el 100% antes de asentarse. Es la base de las animaciones "spring" de muchas apps modernas.

## Herramientas visuales

> [!tip] No los calcules a mano
> Adivinar los cuatro números es difícil. Existen **editores visuales** (cubic-bezier.com, el panel de animaciones de DevTools) donde arrastras los puntos de control y ves la curva y una vista previa. La práctica habitual es diseñar la curva ahí y copiar el `cubic-bezier()` resultante.

## Guardarlas en variables

```css
:root {
  --ease-suave: cubic-bezier(0.4, 0, 0.2, 1);
  --ease-rebote: cubic-bezier(0.34, 1.56, 0.64, 1);
}
.btn { transition: transform 0.3s var(--ease-rebote); }
```

Definir las curvas de marca en variables da una sensación coherente en toda la interfaz.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa un editor visual para diseñar la curva; no adivines los números.
> - `y` fuera de [0,1] para rebote/overshoot (sensación "spring").
> - Guarda las curvas propias en variables y reutilízalas.
> - Para casos simples, las predefinidas (`ease-out`) bastan; reserva `cubic-bezier` para carácter propio.

## Errores comunes

> [!warning] Trampas
> - **Calcular los números a mano**: usa una herramienta.
> - **Rebote exagerado** que distrae más que aporta.
> - **`x` fuera de [0,1]**: inválido (el tiempo no puede retroceder); solo `y` puede salirse.

## Notas relacionadas

- [[01 Curvas (ease, linear, ease-in-out)]] — las predefinidas, casos de `cubic-bezier`.
- [[02 steps()]] — la otra familia (saltos).
- [[03 transition-timing-function]] — donde se aplican.
