---
title: animation-direction — Ida, vuelta, alterna
aliases:
  - animation-direction
  - alternate
tags:
  - css
  - api/propiedad
  - animacion
propiedad: animation-direction
grupo: animacion
valor_inicial: normal
hereda: false
animable: false
draft: false
---

# animation-direction

> [!definicion]
> `animation-direction` decide en qué **sentido** se reproduce la animación en cada repetición: hacia adelante (`normal`), hacia atrás (`reverse`), o **alternando** ida y vuelta (`alternate`). Es clave para bucles que deben "respirar" suavemente en vez de saltar al reiniciar.

```css
.respira { animation: pulso 2s ease-in-out infinite alternate; }
```

## Valores

| Valor | Comportamiento en cada ciclo |
|-------|------------------------------|
| `normal` | De 0% a 100% siempre (por defecto) |
| `reverse` | De 100% a 0% siempre (al revés) |
| `alternate` | Alterna: ida, vuelta, ida, vuelta… |
| `alternate-reverse` | Como alternate, empezando al revés |

## alternate: el bucle que va y vuelve

> [!tip] alternate evita el salto del bucle
> El valor más útil es `alternate`: en vez de saltar del 100% de vuelta al 0% (un corte brusco), la animación **invierte** y vuelve suavemente. Permite definir solo "la ida" en los keyframes y obtener la vuelta gratis:
> ```css
> @keyframes flotar { from { transform: translateY(0); } to { transform: translateY(-10px); } }
> .icono { animation: flotar 2s ease-in-out infinite alternate; }
> /* sube en un ciclo, baja en el siguiente, sin saltos */
> ```
> Sin `alternate`, el icono saltaría de -10px a 0 de golpe al reiniciar; con `alternate`, baja suavemente. Es la forma limpia de hacer efectos de "respiración".

## La curva también se invierte

> [!info] En alternate, el easing se refleja
> Cuando la animación va "de vuelta" (en `alternate`), la `timing-function` también se **invierte**: un `ease-out` de ida se comporta como `ease-in` de vuelta. Esto hace que el movimiento de ida y vuelta sea **simétrico** y natural, sin que tengas que ajustarlo.

## reverse: reproducir al revés

`reverse` reproduce los keyframes de fin a inicio en **todos** los ciclos. Útil cuando quieres el efecto contrario al definido sin reescribir la `@keyframes`:

```css
animation: deslizar 0.4s reverse;   /* sale en vez de entrar */
```

## Combinar con iteration-count

`alternate` brilla con [[04 animation-iteration-count | `infinite`]] (respiración continua) o con un número **par** de iteraciones (para que termine donde empezó). Un número impar con `alternate` termina en el estado "ida".

## Buenas prácticas

> [!tip] Recomendaciones
> - `alternate` + `infinite` para efectos de "respiración" suaves (flotar, pulsar).
> - Define solo la "ida" en los keyframes y deja que `alternate` haga la vuelta.
> - `reverse` para reproducir una animación existente al revés sin reescribirla.
> - Recuerda que en `alternate` el easing se invierte automáticamente (movimiento simétrico).

## Errores comunes

> [!warning] Trampas
> - **Bucle con salto** por no usar `alternate` cuando el fin ≠ inicio.
> - **Número impar de iteraciones con `alternate`**: termina en el estado contrario al esperado.
> - **Reescribir la animación "de vuelta"** en vez de usar `alternate`.

## Notas relacionadas

- [[04 animation-iteration-count]] — el bucle que `alternate` suaviza.
- [[03 animation-timing-function y delay]] — la curva que se invierte en `alternate`.
- [[06 animation-fill-mode]] — el estado al terminar.
