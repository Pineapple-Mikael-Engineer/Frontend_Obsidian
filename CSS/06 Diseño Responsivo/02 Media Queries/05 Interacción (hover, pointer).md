---
title: Interacción — hover, pointer (ratón vs. táctil)
aliases:
  - hover media query
  - pointer media query
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 5
---

# Interacción (hover, pointer)

> [!definicion]
> Las features de **interacción** detectan **cómo** apunta el usuario: con un ratón preciso o con el dedo. `hover` indica si el dispositivo puede "pasar el cursor" y `pointer` la **precisión** del puntero. Permiten adaptar la interfaz a táctil vs. escritorio, algo que el ancho no revela.

```css
@media (hover: hover) and (pointer: fine) {
  .boton:hover { background: #cba6f7; }   /* solo donde el hover tiene sentido */
}
```

## Las features

| Feature | Valores | Significa |
|---------|---------|-----------|
| `hover` | `hover` / `none` | Si el dispositivo puede pasar el cursor (hover real) |
| `pointer` | `fine` / `coarse` / `none` | Precisión del puntero |
| `any-hover` / `any-pointer` | igual | Si **algún** dispositivo de entrada lo cumple |

## El problema que resuelven

> [!info] El ancho no dice si es táctil
> Un error común es asumir "pantalla pequeña = táctil, grande = ratón". Falso: hay tablets grandes táctiles, portátiles con pantalla táctil, y móviles conectados a un ratón. El **ancho no revela el tipo de entrada**. `hover` y `pointer` sí: detectan la capacidad real de apuntar, independiente del tamaño.

## hover: cuándo el :hover tiene sentido

> [!tip] Reservar efectos hover para quien puede hacer hover
> En táctil **no hay hover** real (no hay cursor que pase por encima); un `:hover` en móvil se "pega" tras tocar, dando comportamientos raros. `@media (hover: hover)` aplica los efectos de hover **solo** donde funcionan:
> ```css
> /* El efecto hover solo en dispositivos con cursor */
> @media (hover: hover) {
>   .tarjeta:hover { transform: translateY(-4px); }
> }
> ```
> Así el móvil no hereda estados hover "pegajosos".

## pointer: el tamaño de los objetivos táctiles

> [!tip] Targets más grandes en táctil
> `pointer: coarse` (dedo) indica que el usuario apunta con **baja precisión**, así que los objetivos clicables deben ser **más grandes** (mínimo ~44px). `pointer: fine` (ratón/lápiz) permite objetivos más pequeños:
> ```css
> @media (pointer: coarse) {
>   .boton { min-height: 44px; padding: 0.75rem 1.25rem; }   /* cómodo para el dedo */
> }
> ```
> Es una mejora de usabilidad real para usuarios táctiles.

## any-hover y any-pointer

`hover`/`pointer` se refieren al dispositivo de entrada **principal**; `any-hover`/`any-pointer` consultan si **alguno** de los disponibles cumple. Útil en dispositivos híbridos (un portátil táctil con ratón): `any-hover: hover` es verdadero si **puede** hacer hover con alguno.

## Buenas prácticas

> [!tip] Recomendaciones
> - Aplica efectos `:hover` dentro de `@media (hover: hover)` para que no se "peguen" en táctil.
> - Aumenta el tamaño de los objetivos en `@media (pointer: coarse)`.
> - No deduzcas el tipo de entrada del **ancho**: usa `hover`/`pointer`.
> - Considera `any-pointer`/`any-hover` para dispositivos híbridos.

## Errores comunes

> [!warning] Trampas
> - **Hover "pegajoso"** en móvil por no envolverlo en `@media (hover: hover)`.
> - **Asumir táctil por el ancho**: hay pantallas táctiles grandes y móviles con ratón.
> - **Objetivos pequeños** en táctil: difíciles de tocar (usa `pointer: coarse`).

## Notas relacionadas

- [[01 hover]] — la pseudoclase `:hover`.
- [[04 Pseudoclases de Interacción/index]] — los estados de interacción.
- [[06 Preferencias (prefers-color-scheme, prefers-reduced-motion)]] — otras consultas de adaptación.
