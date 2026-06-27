---
title: animation-fill-mode — El estado antes y después
aliases:
  - animation-fill-mode
  - forwards
tags:
  - css
  - api/propiedad
  - animacion
propiedad: animation-fill-mode
grupo: animacion
valor_inicial: none
hereda: false
animable: false
draft: false
order: 6
---

# animation-fill-mode

> [!definicion]
> `animation-fill-mode` controla qué estilos aplica el elemento **antes** de que empiece la animación (durante el delay) y **después** de que termine. Por defecto (`none`), el elemento "salta" a su estilo normal al acabar; `forwards` lo deja en el **último fotograma**, evitando ese salto.

```css
.entra { animation: aparecer 0.5s forwards; }   /* se queda en el estado final */
```

## El problema: el salto al terminar

> [!warning] Sin fill-mode, el elemento vuelve a su estilo base
> Por defecto (`none`), cuando una animación termina, el elemento **descarta** los estilos de la animación y vuelve a su estilo normal (el del CSS base). Esto causa un "salto" molesto: una animación que desvanece un elemento (`opacity: 1 → 0`) lo deja... **visible** otra vez al terminar, porque vuelve al `opacity` base.
> ```css
> @keyframes desaparecer { to { opacity: 0; } }
> .x { animation: desaparecer 0.5s; }            /* ❌ se desvanece y reaparece */
> .x { animation: desaparecer 0.5s forwards; }   /* ✅ se queda invisible */
> ```

## Los valores

| Valor | Antes (durante el delay) | Después (al terminar) |
|-------|--------------------------|------------------------|
| `none` | Estilo base | Estilo base (salta) |
| `forwards` | Estilo base | **Mantiene el último fotograma** |
| `backwards` | **Aplica el primer fotograma** | Estilo base |
| `both` | Primer fotograma | Último fotograma |

## forwards: quedarse en el final

> [!tip] forwards para animaciones de un solo uso
> `forwards` es el valor más útil para animaciones que **terminan en un estado distinto** del inicial (una entrada, un desvanecimiento, un despliegue). Mantiene el último fotograma, así que el elemento se queda donde la animación lo dejó:
> ```css
> @keyframes entrar { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: none; } }
> .card { animation: entrar 0.5s ease-out forwards; }   /* se queda visible y en su sitio */
> ```

## backwards: aplicar el inicio durante el delay

`backwards` aplica el **primer fotograma** durante el `animation-delay`, antes de que la animación arranque. Sin él, durante el delay el elemento muestra su estilo base, y al empezar "salta" al primer fotograma. Útil con delays para evitar un parpadeo inicial.

## both: lo mejor de ambos

`both` combina `backwards` (estado inicial durante el delay) y `forwards` (estado final al terminar). Es la opción **más segura** para animaciones de un solo uso con delay, evitando saltos en ambos extremos:

```css
.card { animation: entrar 0.5s ease-out 0.2s both; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - `forwards` para que una animación de entrada/salida **se quede** en su estado final.
> - `both` para animaciones con delay (evita saltos al inicio **y** al final).
> - Recuerda: sin fill-mode, el elemento vuelve a su estilo base al terminar.
> - Para estados que deben persistir de verdad, considera también añadir una clase con JS.

## Errores comunes

> [!warning] Trampas
> - **Animación que "se deshace"** al terminar por falta de `forwards`.
> - **Parpadeo durante el delay** por falta de `backwards`/`both`.
> - **Confiar solo en `forwards`** para estados permanentes (si la animación se quita, el estado se pierde; usa una clase para estado real).

## Notas relacionadas

- [[03 animation-timing-function y delay]] — el delay donde `backwards` actúa.
- [[02 animation-name y duration]] — la animación cuyo final mantiene `forwards`.
- [[05 Propiedades Animables]] — qué estado se mantiene.
