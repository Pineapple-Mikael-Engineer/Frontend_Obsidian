---
title: background-attachment — Fondo fijo o desplazable
aliases:
  - background-attachment
tags:
  - css
  - api/propiedad
  - fondos
propiedad: background-attachment
grupo: fondo
valor_inicial: scroll
hereda: false
animable: false
draft: false
order: 6
---

# background-attachment

> [!definicion]
> `background-attachment` controla si la imagen de fondo se **desplaza con el contenido** al hacer scroll o permanece **fija** respecto a la ventana. Su valor `fixed` crea el clásico efecto de "fondo fijo" (un tipo de parallax simple).

```css
.seccion { background-attachment: fixed; }
```

## Valores

| Valor | Comportamiento |
|-------|----------------|
| `scroll` | El fondo se mueve con el elemento al hacer scroll (por defecto) |
| `fixed` | El fondo queda **fijo** respecto a la ventana (el contenido se desliza por encima) |
| `local` | El fondo se desplaza con el contenido **interno** del elemento (si tiene scroll propio) |

## El efecto de fondo fijo (parallax simple)

> [!tip] Fixed para un parallax básico
> `fixed` crea un efecto vistoso: el fondo permanece quieto mientras el contenido se desliza sobre él, como si se viera a través de una "ventana". Es un parallax sencillo sin JavaScript:
> ```css
> .hero-fijo {
>   background: url("paisaje.jpg") center / cover fixed;
>   min-height: 100vh;
> }
> ```
> Al hacer scroll, el texto sube y el paisaje se mantiene, dando profundidad.

## Los problemas de fixed

> [!warning] fixed tiene costes de rendimiento y bugs en móvil
> `background-attachment: fixed` es atractivo pero problemático:
> - **Rendimiento**: obliga al navegador a repintar el fondo en cada frame del scroll, lo que puede causar tirones, sobre todo en imágenes grandes.
> - **Móvil**: muchos navegadores móviles **ignoran** `fixed` (lo tratan como `scroll`) por rendimiento, así que el efecto no funciona donde más usuarios hay.
>
> Para parallax serio, hoy se prefieren técnicas con `transform` o las [[01 scroll-timeline | animaciones de scroll]], más fluidas y fiables.

## local: para elementos con scroll interno

`local` es de nicho: en un elemento con su propio scroll (`overflow: auto`), hace que el fondo se desplace **con el contenido interno**, no con el elemento. Útil para indicar visualmente que un panel tiene más contenido del que se ve.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `fixed` con moderación; ten en cuenta que falla en muchos móviles.
> - Para parallax robusto, prefiere `transform` o scroll-driven animations.
> - `local` para fondos que deben seguir el scroll interno de un panel.
> - Si usas `fixed`, prueba el rendimiento en móvil y con imágenes optimizadas.

## Errores comunes

> [!warning] Trampas
> - **Confiar en `fixed` en móvil**: a menudo se ignora.
> - **`fixed` con imágenes enormes**: tirones al hacer scroll.
> - **Esperar parallax avanzado** de `fixed`: solo da un efecto simple.

## Notas relacionadas

- [[01 scroll-timeline]] — animaciones de scroll modernas para parallax.
- [[06 background-attachment]] — (esta nota).
- [[08 Shorthand background]] — incluir `attachment` en el shorthand.
