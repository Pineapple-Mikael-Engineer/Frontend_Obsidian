---
title: "Animaciones (@keyframes) — Secuencias con fotogramas"
aliases:
  - keyframes
  - css animations
tags:
  - css
  - api/concepto
  - animacion
draft: false
---

# Animaciones (@keyframes)

> [!definicion]
> Las **animaciones** definen una **secuencia** de estados (fotogramas) que un elemento recorre a lo largo del tiempo, de forma autónoma (sin un disparador como el hover). Se definen con la regla `@keyframes` (los fotogramas) y se aplican con las propiedades `animation-*`. Son más potentes que las transiciones: varios pasos, repetición, ida y vuelta.

```css
@keyframes pulso {
  0%   { transform: scale(1); }
  50%  { transform: scale(1.1); }
  100% { transform: scale(1); }
}
.boton { animation: pulso 1.5s ease-in-out infinite; }
```

## Animación vs. transición

> [!info] Secuencia autónoma vs. cambio entre dos estados
> - Una [[01 Transiciones (transition)/index | transición]] va de A a B cuando algo **cambia** (hover, clase). Dos estados, reactiva.
> - Una **animación** recorre una **secuencia** definida (varios fotogramas), puede **repetirse**, ir y volver, y arrancar **sola** al cargar (sin disparador). Más pasos y más control.
>
> Para un spinner que gira en bucle, una entrada elaborada de varios pasos, o un "latido" continuo, animación. Para un hover suave, transición.

## Mapa de la subsección

- [[01 Definición de Fotogramas]] — la regla `@keyframes` y sus porcentajes.
- [[02 animation-name y duration]] — qué animación y cuánto dura.
- [[03 animation-timing-function y delay]] — la curva y el retardo.
- [[04 animation-iteration-count]] — cuántas veces se repite.
- [[05 animation-direction]] — ida, vuelta, alterna.
- [[06 animation-fill-mode]] — el estado antes y después.
- [[07 animation-play-state]] — pausar/reanudar.
- [[08 Múltiples Animaciones]] — varias a la vez.

## El shorthand animation

`animation` combina todas las propiedades:

```css
animation: <name> <duration> <timing> <delay> <count> <direction> <fill-mode> <play-state>;
animation: pulso 1.5s ease-in-out 0s infinite alternate both;
```

El **nombre** y la **duración** son lo esencial; el resto tiene valores por defecto. Por legibilidad, muchos prefieren el shorthand para name/duration/timing y las propiedades sueltas para el resto.

## Las dos piezas: @keyframes y animation

> [!info] Definir y aplicar, separados
> Una animación tiene dos partes independientes:
> 1. **`@keyframes nombre { … }`**: define **qué** ocurre (los estados a lo largo del 0%–100%). Reutilizable.
> 2. **`animation: nombre …`**: **aplica** esa secuencia a un elemento, con su duración, repetición, etc.
>
> La misma `@keyframes` puede aplicarse a varios elementos con distintas duraciones. Detalle en [[01 Definición de Fotogramas]].

## Notas relacionadas

- [[01 Definición de Fotogramas]] — el punto de partida.
- [[08 Múltiples Animaciones]] — combinar varias.
- [[03 Animar transform y opacity]] — el rendimiento de las animaciones.
