---
title: steps() — Animación en saltos discretos
aliases:
  - steps
  - step-end
tags:
  - css
  - api/funcion
  - animacion
draft: false
order: 2
---

# steps()

> [!definicion]
> `steps(n)` divide una transición o animación en **`n` saltos discretos** en vez de un movimiento continuo: el valor cambia de golpe en cada paso, sin valores intermedios. Es la función para **animación de sprites**, efectos de "máquina de escribir" y relojes con segundero a saltos.

```css
animation-timing-function: steps(4);
```

## El uso estrella: sprites

> [!tip] Animar una hoja de sprites
> El caso clásico: una imagen con varios fotogramas en fila (sprite sheet). `steps()` salta de un fotograma al siguiente sin interpolar, creando una animación "frame a frame":
> ```css
> .sprite {
>   width: 64px; height: 64px;
>   background: url("walk.png");   /* 8 fotogramas de 64px = 512px de ancho */
>   animation: andar 0.8s steps(8) infinite;
> }
> @keyframes andar {
>   from { background-position: 0 0; }
>   to   { background-position: -512px 0; }   /* recorre los 8 fotogramas */
> }
> ```
> `steps(8)` hace que el `background-position` salte exactamente entre los 8 fotogramas, sin posiciones intermedias (que mostrarían medio sprite).

## steps con jump

> [!info] Dónde caen los saltos
> `steps()` admite un segundo parámetro que controla **dónde** ocurren los saltos:
> | Valor | Comportamiento |
> |-------|----------------|
> | `steps(n)` o `steps(n, jump-end)` | El salto al final de cada paso (por defecto) |
> | `steps(n, jump-start)` | El salto al inicio de cada paso |
> | `steps(n, jump-none)` | Sin salto en los extremos (n-1 saltos) |
> | `steps(n, jump-both)` | Salto en ambos extremos |
>
> Las palabras `step-start` y `step-end` son atajos de `steps(1, jump-start/end)`.

## El efecto máquina de escribir

> [!tip] Texto que aparece letra a letra
> Combinando `steps()` con una animación de ancho y `overflow: hidden`, se simula texto escribiéndose:
> ```css
> .typewriter {
>   width: 0;
>   overflow: hidden;
>   white-space: nowrap;
>   animation: escribir 2s steps(20) forwards;
> }
> @keyframes escribir { to { width: 20ch; } }
> ```
> `steps(20)` revela el texto carácter a carácter (20 pasos para ~20 caracteres), en vez de un deslizamiento suave.

## Otros usos

- **Reloj con segundero a saltos**: `steps(60)` para 60 posiciones discretas.
- **Barras de progreso "por tramos"**: avanzar en escalones.
- **Cualquier animación que deba verse "digital"** en vez de fluida.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `steps(n)` para sprites: el número de pasos = número de fotogramas.
> - Ajusta `jump-*` si los saltos no caen donde esperas.
> - Para máquina de escribir, `steps(<n.º caracteres>)` con animación de ancho.
> - Para movimiento fluido, NO uses `steps()` (usa curvas continuas).

## Errores comunes

> [!warning] Trampas
> - **Número de pasos incorrecto**: se ven medios fotogramas del sprite.
> - **`jump-end` vs. `jump-start`** mal elegido: el último/primer fotograma se salta.
> - **Usar `steps()` para movimiento suave**: queda a tirones (es para saltos a propósito).

## Notas relacionadas

- [[01 Curvas (ease, linear, ease-in-out)]] — las curvas continuas (lo opuesto).
- [[03 cubic-bezier()]] — curvas a medida.
- [[04 Animaciones (@keyframes)/index]] — las animaciones donde `steps()` brilla.
