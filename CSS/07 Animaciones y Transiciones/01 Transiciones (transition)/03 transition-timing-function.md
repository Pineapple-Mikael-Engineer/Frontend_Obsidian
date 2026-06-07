---
title: transition-timing-function — La curva de aceleración
aliases:
  - transition-timing-function
  - easing
tags:
  - css
  - api/propiedad
  - animacion
propiedad: transition-timing-function
grupo: animacion
valor_inicial: ease
hereda: false
animable: false
draft: false
---

# transition-timing-function

> [!definicion]
> `transition-timing-function` (el *easing*) define **cómo se distribuye** la transición en el tiempo: si empieza rápido y frena, arranca lento y acelera, o avanza a velocidad constante. Es lo que hace que un movimiento se sienta **natural** en vez de mecánico.

```css
.x { transition: transform 0.3s ease-out; }
```

## Por qué el easing importa

> [!tip] El movimiento del mundo real no es lineal
> En la realidad, las cosas **aceleran y frenan**: un objeto no arranca a velocidad máxima de golpe. Una transición `linear` (velocidad constante) se siente **robótica**; una con easing (que arranca o frena suavemente) se siente **natural y agradable**. El easing es lo que separa una animación amateur de una pulida. El detalle de cada curva está en [[02 Funciones de Tiempo/index | Funciones de Tiempo]].

## Las curvas predefinidas

| Valor | Comportamiento | Sensación |
|-------|----------------|-----------|
| `linear` | Velocidad constante | Robótica (útil para giros continuos) |
| `ease` | Acelera y frena (por defecto) | Natural, equilibrada |
| `ease-in` | Arranca lento, acelera al final | "Entra" (bueno para salidas) |
| `ease-out` | Arranca rápido, frena al final | "Sale" (bueno para entradas) |
| `ease-in-out` | Lento, rápido, lento | Suave en ambos extremos |
| `cubic-bezier(...)` | Curva a medida | Total control |
| `steps(n)` | Saltos discretos | Animación "frame a frame" |

## ease-out para entradas, ease-in para salidas

> [!tip] La regla de oro del easing en UI
> Una pauta muy útil:
> - **Entradas** (algo que aparece): `ease-out`. Arranca rápido y frena, dando sensación de que el elemento "llega y se asienta".
> - **Salidas** (algo que desaparece): `ease-in`. Arranca lento y acelera, como si "se fuera".
>
> ```css
> .panel { transition: transform 0.3s ease-out; }   /* al aparecer */
> .panel.saliendo { transition: transform 0.2s ease-in; }
> ```
> `ease-out` es la curva más usada en interfaces porque la respuesta inmediata se siente reactiva.

## cubic-bezier y steps

Para control total, `cubic-bezier(x1, y1, x2, y2)` define una curva a medida (incluso con "rebote"); `steps(n)` divide la transición en saltos discretos (útil para sprites). Detalle en [[03 cubic-bezier()]] y [[02 steps()]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `ease-out` para entradas (lo más común en UI) y `ease-in` para salidas.
> - `ease` (por defecto) es una opción segura y natural para casi todo.
> - `linear` solo para movimientos continuos (un spinner que gira).
> - Para una sensación de marca propia, define una `cubic-bezier` y reúsala en variables.

## Errores comunes

> [!warning] Trampas
> - **`linear` en transiciones de UI**: se siente robótico.
> - **Easing al revés**: `ease-in` en una entrada la hace sentir perezosa.
> - **Demasiado rebote** (`cubic-bezier` exagerado): distrae más que aporta.

## Notas relacionadas

- [[02 Funciones de Tiempo/index]] — el detalle de cada curva.
- [[03 cubic-bezier()]] — curvas a medida.
- [[02 transition-duration]] — la duración que la curva distribuye.
