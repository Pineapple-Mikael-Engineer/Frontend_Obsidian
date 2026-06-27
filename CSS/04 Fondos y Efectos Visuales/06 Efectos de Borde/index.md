---
title: Efectos de Borde — outline y box-decoration-break
aliases:
  - border effects
tags:
  - css
  - api/concepto
  - fondos
draft: false
order: 5
---

# Efectos de Borde

> [!definicion]
> Dos propiedades complementan al [[04 Border/index | borde]] normal: [[01 outline | `outline`]] dibuja un contorno que **no ocupa espacio** (clave para el foco de teclado), y [[02 box-decoration-break | `box-decoration-break`]] controla cómo se aplican bordes, fondos y sombras cuando un elemento en línea **se parte** en varias líneas.

```css
:focus-visible { outline: 2px solid #cba6f7; outline-offset: 2px; }
.resaltado { box-decoration-break: clone; }
```

## Mapa de la subsección

- [[01 outline]] — el contorno que no afecta al layout (foco, depuración).
- [[02 box-decoration-break]] — bordes/fondos en elementos partidos en varias líneas.

## outline: el héroe del foco

> [!tip] outline es la base de la accesibilidad por teclado
> El uso más importante de `outline` es el **anillo de foco**: el contorno que indica qué elemento tiene el foco del teclado. Como no ocupa espacio, no desplaza el layout al aparecer, a diferencia de un `border`. Eliminarlo (`outline: none`) sin reemplazo es uno de los peores errores de accesibilidad. Detalle en [[01 outline]] y en [[04 focus-visible]].

## Notas relacionadas

- [[01 outline]] — el contorno de foco.
- [[04 Border/index]] — el borde normal, que sí ocupa espacio.
- [[04 focus-visible]] — la pseudoclase que decide cuándo mostrar el outline.
