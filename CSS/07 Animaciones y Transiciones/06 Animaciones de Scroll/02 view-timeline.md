---
title: view-timeline — Animación según la visibilidad de un elemento
aliases:
  - view-timeline
  - "view()"
tags:
  - css
  - api/propiedad
  - animacion
propiedad: animation-timeline
grupo: animacion
valor_inicial: auto
hereda: false
animable: false
draft: false
order: 2
---

# view-timeline

> [!definicion]
> Una **view timeline** liga el progreso de una animación a la **visibilidad** de un elemento en el viewport: la animación avanza a medida que el elemento **entra y atraviesa** la pantalla. Es el reemplazo nativo del clásico "revelar al hacer scroll" (fade-in, deslizar) que antes requería `IntersectionObserver`.

```css
.revelar {
  animation: aparecer linear;
  animation-timeline: view();
  animation-range: entry;   /* mientras el elemento entra en pantalla */
}
@keyframes aparecer { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: none; } }
```

## La función view()

`animation-timeline: view()` liga la animación a cuánto del elemento es **visible** en el viewport. La animación va del 0% (el elemento aún no ha entrado) al 100% (lo ha atravesado por completo).

## animation-range: dónde ocurre la animación

> [!tip] Acotar la animación a la entrada del elemento
> `animation-range` define **en qué tramo** de la visibilidad ocurre la animación. Lo más común es `entry` (mientras el elemento **entra** en pantalla por abajo):
> | Valor | La animación ocurre… |
> |-------|----------------------|
> | `entry` | Mientras el elemento entra (aparece por abajo) |
> | `exit` | Mientras sale (desaparece por arriba) |
> | `cover` | Durante todo el paso por el viewport |
> | `entry 0% entry 50%` | En un tramo personalizado |
>
> ```css
> animation-timeline: view();
> animation-range: entry 0% entry 100%;   /* anima durante toda la entrada */
> ```

## El "reveal on scroll" sin JavaScript

> [!tip] Revelar elementos al entrar, nativo
> El efecto más popular: que cada sección/tarjeta aparezca (fade-in + deslizar) cuando entra en pantalla. Antes requería `IntersectionObserver` y clases en JS; ahora es CSS puro:
> ```css
> .seccion {
>   animation: revelar linear both;
>   animation-timeline: view();
>   animation-range: entry 0% entry 60%;   /* completa antes de estar del todo visible */
> }
> @keyframes revelar {
>   from { opacity: 0; transform: translateY(40px); }
>   to   { opacity: 1; transform: none; }
> }
> ```
> Cada elemento con esta regla se revela al entrar, sin observadores ni eventos. `both` ([[06 animation-fill-mode | fill-mode]]) mantiene el estado inicial antes y el final después.

## El efecto parallax

`view()` también permite **parallax** (capas que se mueven a distinta velocidad con el scroll) ligando distintas animaciones de `translateY` a la visibilidad de cada capa, de forma eficiente y sin JS.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `view()` + `animation-range: entry` para revelar elementos al entrar en pantalla.
> - Acaba la animación antes del 100% (`entry 60%`) para que el elemento ya esté visible cuando termina.
> - Anima `opacity` + `transform` (eficiente).
> - Respeta `prefers-reduced-motion` y trátalo como mejora progresiva.

## Errores comunes

> [!warning] Trampas
> - **Sin `animation-range`**: la animación abarca todo el paso, a veces no como se espera.
> - **Animaciones que terminan demasiado tarde**: el elemento aún no se ve completo.
> - **Ignorar `prefers-reduced-motion`**: revelar con movimiento puede marear.

## Notas relacionadas

- [[01 scroll-timeline]] — ligar al scroll del contenedor (barras de progreso).
- [[06 animation-fill-mode]] — `both` para mantener el estado.
- [[06 Preferencias (prefers-color-scheme, prefers-reduced-motion)]] — el respeto al movimiento.
