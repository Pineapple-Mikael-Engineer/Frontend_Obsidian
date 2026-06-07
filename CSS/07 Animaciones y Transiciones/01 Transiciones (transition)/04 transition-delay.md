---
title: transition-delay — El retardo antes de empezar
aliases:
  - transition-delay
tags:
  - css
  - api/propiedad
  - animacion
propiedad: transition-delay
grupo: animacion
valor_inicial: "0s"
hereda: false
animable: false
draft: false
---

# transition-delay

> [!definicion]
> `transition-delay` introduce un **retardo** antes de que la transición empiece, tras el cambio que la dispara. Permite escalonar varias transiciones (efecto cascada) o evitar que un cambio reaccione de inmediato.

```css
.x { transition: opacity 0.3s ease 0.1s; }   /* espera 0.1s, luego anima 0.3s */
```

## Escalonar transiciones (stagger)

> [!tip] El efecto cascada con delays crecientes
> El uso más vistoso: dar a varios elementos retardos **crecientes** para que entren uno tras otro, en cascada:
> ```css
> .item { transition: opacity 0.4s, transform 0.4s; }
> .item:nth-child(1) { transition-delay: 0s; }
> .item:nth-child(2) { transition-delay: 0.1s; }
> .item:nth-child(3) { transition-delay: 0.2s; }
> ```
> Con una variable calculada (`--i`) se hace más limpio:
> ```css
> .item { transition-delay: calc(var(--i) * 0.1s); }
> ```
> Los elementos aparecen en secuencia, un efecto muy usado en menús y listas que se revelan.

## Delay negativo

> [!info] Un delay negativo "adelanta" la transición
> Un `transition-delay` **negativo** hace que la transición empiece como si ya llevara un tiempo en marcha: arranca a mitad de camino. Es menos común en transiciones que en animaciones (donde el delay negativo es muy útil para desfasar bucles), pero existe.

## El delay de salida

> [!tip] Retrasar la desaparición (menús con submenú)
> Un uso práctico del delay: en un menú con submenú que aparece en `:hover`, un pequeño delay en la **salida** evita que el submenú desaparezca si el cursor pasa brevemente fuera, dando margen al usuario:
> ```css
> .submenu { transition: opacity 0.2s, visibility 0s 0.2s; }   /* visibility espera */
> .menu:hover .submenu { transition-delay: 0s; }
> ```

## No abusar de los delays

> [!warning] Demasiado retardo frustra
> Un delay hace que la interfaz **tarde en responder**, lo que puede sentirse lento o roto. Para feedback directo (un botón en hover), el delay debe ser **0 o mínimo**. Reserva los delays para efectos deliberados (cascadas, evitar parpadeos), no para todo.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa delays crecientes para efectos en cascada (listas, menús).
> - `calc(var(--i) * <paso>)` para escalonar de forma limpia.
> - Delay 0 en feedback directo (hover de botón): no hagas esperar al usuario.
> - Un delay de salida pequeño para menús que no deben desaparecer al instante.

## Errores comunes

> [!warning] Trampas
> - **Delay en feedback inmediato**: la interfaz se siente lenta o rota.
> - **Cascadas demasiado largas**: el último elemento tarda demasiado en aparecer.
> - **Confundir delay con duración**: el delay es la espera **antes**, la duración el tiempo de la animación.

## Notas relacionadas

- [[02 transition-duration]] — la duración, no confundir con el delay.
- [[03 nth-child()]] · variables para escalonar los delays.
- [[index]] — el shorthand completo.
