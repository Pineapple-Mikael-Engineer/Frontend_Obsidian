---
title: target — El destino de un enlace de ancla
aliases:
  - ":target"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":target"
grupo: pseudoclase
draft: false
---

# :target

> [!definicion]
> `:target` selecciona el elemento cuyo `id` coincide con el **fragmento de la URL** (la parte tras `#`). Cuando se navega a `pagina.html#seccion3`, el elemento con `id="seccion3"` pasa a estar `:target`. Permite reaccionar en CSS a la navegación por anclas, sin JavaScript.

```css
:target { scroll-margin-top: 5rem; }
section:target { background: rgb(203 166 247 / 0.1); }
```

## Resaltar la sección enlazada

> [!tip] Destacar a dónde te ha llevado un enlace
> El uso más natural: resaltar brevemente la sección a la que el usuario ha saltado desde un índice o un enlace de ancla, para orientarle:
> ```css
> section:target {
>   background: rgb(203 166 247 / 0.1);
>   animation: destacar 1s ease;
> }
> @keyframes destacar { from { background: rgb(203 166 247 / 0.4); } }
> ```
> Al hacer clic en un enlace `#seccion`, esa sección se ilumina un momento, ayudando a no perderse.

## scroll-margin para anclas bajo cabeceras fijas

> [!tip] El ancla que queda tapada por la cabecera
> Un problema clásico de los enlaces de ancla con una cabecera **fija**: al saltar a una sección, queda **tapada** por la barra superior. `scroll-margin-top` (a menudo aplicado vía `:target` o al elemento) reserva ese espacio:
> ```css
> :target { scroll-margin-top: 5rem; }   /* deja sitio bajo la cabecera fija */
> ```
> Así la sección enlazada aparece **debajo** de la cabecera, no escondida tras ella.

## Componentes sin JavaScript

> [!info] Pestañas, modales y acordeones con :target
> `:target` permite construir componentes interactivos **sin JS**: pestañas, modales que se abren con un enlace `#modal`, acordeones. Funciona, pero con **limitaciones**:
> - Cada interacción **cambia la URL** (añade `#…`) y crea entradas en el historial.
> - La gestión de foco y los atributos ARIA son pobres comparados con una solución JS.
> - El comportamiento de scroll puede ser inesperado.
>
> Es ingenioso para prototipos o casos muy simples, pero para componentes serios, JavaScript con ARIA es preferible (igual que el truco del checkbox para [[03 Menús Hamburguesa | menús]]).

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:target` para resaltar la sección a la que lleva un enlace de ancla.
> - Combínalo con `scroll-margin-top` para que el ancla no quede bajo una cabecera fija.
> - Para componentes interactivos serios, prefiere JS con ARIA al truco de `:target`.
> - Ten en cuenta que `:target` modifica la URL y el historial.

## Errores comunes

> [!warning] Trampas
> - **Ancla tapada** por una cabecera fija sin `scroll-margin-top`.
> - **Componentes con `:target`** que ensucian el historial y la accesibilidad.
> - **Esperar persistencia**: `:target` cambia si la URL cambia.

## Notas relacionadas

- [[05 Enlaces y Navegación/index]] — los enlaces de ancla que disparan `:target`.
- [[03 Menús Hamburguesa]] — otro patrón "CSS-only" con sus límites.
- [[07 root]] — `:root`, otra pseudoclase estructural.
