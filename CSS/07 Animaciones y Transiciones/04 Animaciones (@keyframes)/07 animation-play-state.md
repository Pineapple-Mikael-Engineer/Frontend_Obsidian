---
title: animation-play-state — Pausar y reanudar
aliases:
  - animation-play-state
tags:
  - css
  - api/propiedad
  - animacion
propiedad: animation-play-state
grupo: animacion
valor_inicial: running
hereda: false
animable: false
draft: false
order: 7
---

# animation-play-state

> [!definicion]
> `animation-play-state` **pausa** o **reanuda** una animación: `running` (corriendo, por defecto) o `paused` (congelada en su posición actual). Permite detener una animación con CSS —en `:hover`, con una clase— sin reiniciarla.

```css
.marquesina { animation: desplazar 10s linear infinite; }
.marquesina:hover { animation-play-state: paused; }   /* se detiene al pasar el ratón */
```

## Pausa sin reiniciar

> [!info] Congela en el sitio, no vuelve al inicio
> A diferencia de quitar la animación (que la reinicia desde 0), `animation-play-state: paused` **congela** la animación en su posición exacta. Al volver a `running`, **continúa** desde donde se quedó. Es una pausa real, no un reinicio.

## Casos de uso

```css
/* Marquesina que se detiene al pasar el ratón (para leer) */
.noticias { animation: scroll 20s linear infinite; }
.noticias:hover { animation-play-state: paused; }

/* Carrusel que se pausa al interactuar */
.carrusel:hover, .carrusel:focus-within {
  animation-play-state: paused;
}

/* Pausa controlada por una clase (toggle con JS) */
.spinner.pausado { animation-play-state: paused; }
```

## Pausar al pasar el ratón

> [!tip] El patrón de la marquesina pausable
> Un patrón clásico de usabilidad: una animación continua (noticias que se desplazan, un carrusel automático) que se **pausa** cuando el usuario pasa el ratón o le da el foco, para poder leer/interactuar sin que se mueva:
> ```css
> .ticker:hover, .ticker:focus-within { animation-play-state: paused; }
> ```
> Mejora mucho la experiencia: el contenido en movimiento no se escapa cuando el usuario quiere fijarse en él.

## Control desde JavaScript

`animation-play-state` se puede alternar desde JS (cambiando la propiedad o una clase) para controles de play/pausa. También se relaciona con la **Web Animations API**, que ofrece control programático completo (`.pause()`, `.play()`, `.currentTime`), más potente para casos complejos.

## Accesibilidad: ofrecer pausa

> [!tip] El contenido en movimiento debe poder pausarse
> Las WCAG (2.2.2) exigen que el contenido que se **mueve automáticamente** durante más de 5 segundos pueda **pausarse**. `animation-play-state` es una forma de cumplirlo (con un botón de pausa o pausando en hover/focus). Combínalo con respetar [[06 Preferencias (prefers-color-scheme, prefers-reduced-motion) | `prefers-reduced-motion`]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Pausa animaciones continuas en `:hover`/`:focus-within` para que el usuario pueda leer/interactuar.
> - Ofrece un control de pausa para contenido en movimiento (requisito WCAG).
> - Recuerda que `paused` congela sin reiniciar (reanuda desde donde quedó).
> - Para control fino programático, considera la Web Animations API.

## Errores comunes

> [!warning] Trampas
> - **Contenido en movimiento sin forma de pausar**: incumple WCAG 2.2.2.
> - **Confundir pausa con reinicio**: `paused` congela, no vuelve al inicio.
> - **Olvidar `:focus-within`** además de `:hover` (accesibilidad por teclado).

## Notas relacionadas

- [[04 animation-iteration-count]] — los bucles que se pausan.
- [[06 Preferencias (prefers-color-scheme, prefers-reduced-motion)]] — el respeto al movimiento.
- [[05 focus-within]] — pausar también con el foco de teclado.
