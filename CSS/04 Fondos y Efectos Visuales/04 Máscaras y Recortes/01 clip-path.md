---
title: clip-path — Recortar con una forma
aliases:
  - clip-path
tags:
  - css
  - api/propiedad
  - fondos
propiedad: clip-path
grupo: fondo
valor_inicial: none
hereda: false
animable: true
draft: false
order: 1
---

# clip-path

> [!definicion]
> `clip-path` **recorta** un elemento a una **forma** geométrica: lo que queda dentro de la forma se ve, lo de fuera desaparece. Permite crear hexágonos, círculos, flechas, diagonales y cualquier polígono sin imágenes ni SVG. El recorte es nítido (todo o nada).

```css
.circulo { clip-path: circle(50%); }
.diagonal { clip-path: polygon(0 0, 100% 0, 100% 80%, 0 100%); }
```

## Las funciones de forma

| Función | Forma | Ejemplo |
|---------|-------|---------|
| `circle(r at x y)` | Círculo | `circle(50%)` |
| `ellipse(rx ry at x y)` | Elipse | `ellipse(40% 50%)` |
| `inset(arriba der abajo izq)` | Rectángulo recortado (con radio opcional) | `inset(10px round 8px)` |
| `polygon(x1 y1, x2 y2, ...)` | Polígono de puntos | ver abajo |
| `path("...")` | Trazado SVG arbitrario | curvas complejas |

## polygon: la más versátil

> [!info] Puntos en porcentaje de la caja
> `polygon()` define una forma con una lista de puntos `x y`, normalmente en **porcentaje** del tamaño de la caja (`0 0` = esquina superior izquierda, `100% 100%` = inferior derecha). Los puntos se conectan en orden:
> ```css
> /* Triángulo apuntando arriba */
> clip-path: polygon(50% 0, 100% 100%, 0 100%);
>
> /* Hexágono */
> clip-path: polygon(25% 0, 75% 0, 100% 50%, 75% 100%, 25% 100%, 0 50%);
>
> /* Esquina cortada (chaflán) */
> clip-path: polygon(0 0, calc(100% - 20px) 0, 100% 20px, 100% 100%, 0 100%);
> ```
> Herramientas visuales (clip-path makers) ayudan a generar los puntos.

## Recetas comunes

```css
/* Avatar circular (alternativa a border-radius: 50%) */
.avatar { clip-path: circle(50%); }

/* Sección con borde diagonal */
.seccion-diagonal { clip-path: polygon(0 0, 100% 0, 100% 90%, 0 100%); }

/* Flecha / cinta */
.cinta { clip-path: polygon(0 0, 100% 0, 90% 50%, 100% 100%, 0 100%); }
```

## Animar la forma

> [!tip] clip-path es animable (con cuidado)
> `clip-path` se puede **animar** y transicionar, lo que da efectos de revelado o cambio de forma. La condición: para transicionar entre dos `polygon`, ambos deben tener el **mismo número de puntos** (el navegador interpola punto a punto):
> ```css
> .reveal { clip-path: inset(0 100% 0 0); transition: clip-path 0.5s; }
> .reveal.visible { clip-path: inset(0 0 0 0); }   /* se "abre" de izquierda a derecha */
> ```

## clip-path vs. border-radius vs. overflow

| Quiero… | Usar |
|---------|------|
| Esquinas redondeadas simples | `border-radius` |
| Una forma geométrica (hexágono, diagonal) | `clip-path` |
| Recortar contenido desbordado a la caja | `overflow: hidden` |
| Transparencia **gradual** (desvanecido) | `mask` |

Para esquinas simples, [[05 border-radius]]; para transparencia gradual, [[02 mask]].

## El área clicable se recorta

> [!info] El recorte afecta también a los eventos
> A diferencia de un fondo con forma simulada, `clip-path` recorta **de verdad** el elemento: las zonas fuera de la forma **no reciben clics ni hover**. Esto es coherente (la flecha solo responde donde se ve) pero hay que tenerlo en cuenta si esperabas que toda la caja rectangular fuera clicable.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `clip-path` para formas geométricas (hexágonos, diagonales, flechas) sin imágenes.
> - Para esquinas redondeadas simples, `border-radius` es más fácil.
> - Para transiciones, mantén el mismo número de puntos entre las dos formas.
> - Recuerda que el recorte afecta al área clicable.
> - Usa generadores visuales para los `polygon()` complejos.

## Errores comunes

> [!warning] Trampas
> - **Transición entre polígonos con distinto número de puntos**: no interpola.
> - **Esperar clics fuera de la forma**: el recorte quita la interactividad ahí.
> - **Formas con muchos puntos** a mano: usa una herramienta visual.

## Notas relacionadas

- [[02 mask]] — la alternativa con transparencia gradual.
- [[05 border-radius]] — recorte de esquinas simple.
- [[01 Mapa de Imagen (map, area)]] — zonas clicables, comparado con el recorte de eventos.
