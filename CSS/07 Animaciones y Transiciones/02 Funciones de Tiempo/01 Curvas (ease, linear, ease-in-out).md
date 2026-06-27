---
title: Curvas predefinidas — ease, linear, ease-in, ease-out, ease-in-out
aliases:
  - ease
  - linear
  - ease-in-out
tags:
  - css
  - api/concepto
  - animacion
draft: false
order: 1
---

# Curvas (ease, linear, ease-in-out)

> [!definicion]
> CSS trae cinco curvas de aceleración con nombre, que cubren los casos más comunes: `linear` (constante), `ease` (la equilibrada por defecto), `ease-in` (acelera), `ease-out` (frena) y `ease-in-out` (suave en ambos extremos). Son atajos de [[03 cubic-bezier() | curvas `cubic-bezier`]] concretas.

```css
transition-timing-function: ease-out;
```

## Las cinco curvas

| Curva | Comportamiento | Sensación |
|-------|----------------|-----------|
| `linear` | Velocidad **constante** | Mecánica, robótica |
| `ease` | Acelera al inicio, frena al final (suave) | Natural (por defecto) |
| `ease-in` | Arranca **lento**, acelera | "Coge impulso" |
| `ease-out` | Arranca **rápido**, frena | "Llega y se asienta" |
| `ease-in-out` | Lento → rápido → lento | Muy suave |

## Visualizar las curvas

```
linear:       ────────────────  (recta)
ease-out:     ──────╮          (rápido y luego frena)
ease-in:           ╭──────     (lento y luego acelera)
ease-in-out:  ──╮      ╭──     (frena en ambos extremos)
```

## Cuándo cada una

> [!tip] La guía práctica
> - **`ease-out`**: la curva más usada en UI. Para **entradas** y respuestas a la interacción: el elemento reacciona de inmediato (arranque rápido) y se asienta suavemente. Hover, paneles que aparecen.
> - **`ease-in`**: para **salidas**, donde el elemento "se va" acelerando. Combinado con `ease-out` para entrada da movimientos coherentes.
> - **`ease-in-out`**: para movimientos que van de un sitio a otro y se detienen en ambos (un carrusel que se desliza). Muy suave pero puede sentirse "lento" al arrancar.
> - **`linear`**: solo para movimientos **continuos** sin inicio/fin perceptible (un spinner que gira, una marquesina). En transiciones de UI normales, se siente robótico.
> - **`ease`** (por defecto): una opción segura y natural cuando no quieres pensarlo.

## linear: el malentendido

> [!warning] linear se siente robótico en UI
> `linear` es tentador por "simple", pero en transiciones de interfaz se percibe **antinatural**: nada en el mundo real se mueve a velocidad perfectamente constante. Reserva `linear` para giros continuos (spinners) o barras de progreso, donde la velocidad constante **sí** tiene sentido. Para todo lo demás, una curva con easing.

## Más allá de las cinco

Para curvas con más carácter (rebote, "overshoot", anticipación), las cinco predefinidas no bastan: hace falta [[03 cubic-bezier() | `cubic-bezier()`]] a medida. Las cinco cubren lo básico; el control fino viene de las curvas personalizadas.

## Buenas prácticas

> [!tip] Recomendaciones
> - `ease-out` para entradas y respuestas a interacción (lo más usado).
> - `ease-in` para salidas; combina ambas para movimientos coherentes.
> - `linear` solo para movimientos continuos (spinners, progreso).
> - `ease` por defecto si no quieres decidir; `cubic-bezier` para carácter propio.

## Errores comunes

> [!warning] Trampas
> - **`linear` en transiciones de UI**: robótico.
> - **Easing al revés**: `ease-in` en una entrada la hace sentir lenta.
> - **Usar siempre la misma curva** sin pensar entrada vs. salida.

## Notas relacionadas

- [[03 cubic-bezier()]] — curvas a medida (rebote, etc.).
- [[02 steps()]] — la otra familia (saltos).
- [[03 transition-timing-function]] — aplicar las curvas en transiciones.
