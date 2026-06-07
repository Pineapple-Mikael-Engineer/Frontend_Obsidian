---
title: will-change — Avisar al navegador de qué se va a animar
aliases:
  - will-change
tags:
  - css
  - api/propiedad
  - animacion
propiedad: will-change
grupo: animacion
valor_inicial: auto
hereda: false
animable: false
draft: false
---

# will-change

> [!definicion]
> `will-change` **avisa** al navegador de que una propiedad va a cambiar pronto, para que la **prepare** por adelantado (por ejemplo, promoviendo el elemento a su propia capa de composición de la GPU). Bien usada, hace las animaciones más fluidas; mal usada, **empeora** el rendimiento.

```css
.que-va-a-animar { will-change: transform; }
```

## Qué hace

> [!info] Promover a capa de GPU, anticipadamente
> Sin `will-change`, el navegador prepara la optimización (crear una capa de composición) **en el momento** en que la animación empieza, lo que puede causar un pequeño tirón inicial. `will-change` le dice "prepárate, esto va a cambiar", para que la capa esté lista **antes**:
> ```css
> .modal { will-change: transform, opacity; }
> ```
> Es como reservar recursos por adelantado para una animación que sabes que viene.

## La regla de oro: úsalo con moderación

> [!warning] will-change en exceso empeora el rendimiento
> El error más común es abusar de `will-change`. Cada elemento con `will-change` consume **memoria** (crea una capa de composición permanente en la GPU). Aplicarlo a muchos elementos —o dejarlo siempre activo— **agota la memoria** y **ralentiza** todo, el efecto contrario al buscado:
> ```css
> /* ❌ NUNCA esto: todas las capas permanentes */
> * { will-change: transform; }
> ```
> `will-change` es una herramienta de **último recurso** para una animación concreta que va a tirones, no un "acelerador" para aplicar por todas partes.

## Aplicarlo y quitarlo

> [!tip] Activa antes, quita después
> La práctica recomendada: activar `will-change` **justo antes** de la animación (p. ej. en `:hover` del padre, o con JS al iniciar) y **quitarlo** al terminar, para no mantener la capa indefinidamente:
> ```css
> .card-wrap:hover .card { will-change: transform; }   /* se activa al acercarse */
> .card { transition: transform 0.3s; }
> .card-wrap:hover .card { transform: scale(1.05); }
> ```
> Activarlo en el `:hover` del contenedor lo prepara un instante antes de la animación real, sin dejarlo permanente.

## No lo necesitas casi nunca

> [!info] Empieza sin will-change
> La mayoría de las animaciones de `transform`/`opacity` **ya** son fluidas sin `will-change` (el navegador las optimiza bien). Solo recurre a él si **mides** un tirón concreto que no se arregla de otra forma. Primero anima propiedades baratas; `will-change` es el ajuste fino final, no el punto de partida.

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsalo solo en elementos concretos con un problema de rendimiento **medido**.
> - Actívalo justo antes de la animación y quítalo al terminar.
> - **Nunca** lo apliques globalmente (`*`) ni lo dejes permanente en muchos elementos.
> - Primero anima `transform`/`opacity` (ya suelen ser fluidos); `will-change` es el último recurso.

## Errores comunes

> [!warning] Trampas
> - **Abusar de `will-change`**: agota la memoria de la GPU y ralentiza todo.
> - **Dejarlo permanente** en muchos elementos.
> - **Usarlo como "acelerador" mágico** sin medir si hace falta.

## Notas relacionadas

- [[01 Layout, Paint y Composición]] — la capa de composición que `will-change` prepara.
- [[03 Animar transform y opacity]] — las animaciones que suelen ir bien sin `will-change`.
- [[03 Transformaciones 3D (translateZ, rotateX, scale3d)]] — el viejo hack `translateZ(0)` que `will-change` reemplaza.
