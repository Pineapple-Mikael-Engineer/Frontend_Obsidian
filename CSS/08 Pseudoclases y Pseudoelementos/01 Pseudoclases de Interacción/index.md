---
title: Pseudoclases de Interacción — Estados del usuario
aliases:
  - interaction pseudo-classes
tags:
  - css
  - api/concepto
  - selectores
draft: false
---

# Pseudoclases de Interacción

> [!definicion]
> Las pseudoclases de **interacción** seleccionan elementos según cómo el usuario interactúa con ellos: pasar el ratón (`:hover`), pulsar (`:active`), tener el foco (`:focus`, `:focus-visible`), o ser el destino de un enlace (`:target`). Son la base del feedback visual de una interfaz.

```css
a:hover { color: #cba6f7; }
button:active { transform: scale(0.97); }
input:focus-visible { outline: 2px solid #cba6f7; }
```

## Mapa de la subsección

- [[01 hover]] — el ratón por encima.
- [[02 active]] — mientras se pulsa.
- [[03 focus]] — el elemento enfocado.
- [[04 focus-visible]] — foco solo por teclado.
- [[05 focus-within]] — un ancestro con un descendiente enfocado.
- [[06 target]] — el destino de un enlace de ancla.
- [[07 visited]] — enlaces ya visitados.

## El orden importa: LVHA

> [!warning] Las pseudoclases de enlace tienen un orden obligatorio
> Para los enlaces, las cuatro pseudoclases de estado deben declararse en un **orden concreto** o se pisan entre sí (por la especificidad igual y la cascada). La regla mnemotécnica es **LVHA** ("LoVe HAte"):
> ```css
> a:link    { } /* 1. sin visitar */
> a:visited { } /* 2. visitado */
> a:hover   { } /* 3. ratón encima */
> a:active  { } /* 4. pulsando */
> ```
> Si pones `:hover` antes que `:visited`, un enlace visitado no mostraría el efecto hover. El orden LVHA garantiza que cada estado se vea cuando toca.

## El foco es accesibilidad, no decoración

> [!tip] :focus-visible es imprescindible
> De todas estas, las de **foco** son las más importantes para la accesibilidad: marcan dónde está el usuario que navega con teclado. Nunca elimines el indicador de foco sin reemplazo. La forma moderna es [[04 focus-visible | `:focus-visible`]] (foco solo con teclado), que evita el "anillo persistente" tras un clic de ratón. Detalle en su nota y en [[01 outline]].

## Notas relacionadas

- [[01 hover]] — la más común.
- [[04 focus-visible]] — el foco accesible.
- [[05 Interacción (hover, pointer)]] — detectar si el dispositivo puede hacer hover.
