---
title: border-color — Color del borde
aliases:
  - border-color
tags:
  - css
  - api/propiedad
  - layout
propiedad: border-color
grupo: box-model
valor_inicial: currentColor
hereda: false
animable: true
draft: false
order: 3
---

# border-color

> [!definicion]
> `border-color` define el **color** del borde. Su valor inicial es `currentColor`, lo que significa que, si no se especifica, el borde toma el **color del texto** del elemento. Acepta cualquier [[05 Colores/index | formato de color]].

```css
.caja { border: 2px solid; border-color: #cba6f7; }
```

## El valor por defecto: currentColor

> [!tip] El borde sigue al color del texto por defecto
> Si defines un borde sin color, toma `currentColor` (el valor de [[01 color | `color`]]). Esto es muy útil: el borde se **sincroniza** con el texto sin repetir el color:
> ```css
> .chip {
>   color: #cba6f7;
>   border: 2px solid;   /* sin color: usa currentColor → mismo morado */
> }
> .chip:hover { color: #f38ba8; }   /* el borde cambia solo */
> ```
> Cambiar `color` actualiza texto **y** borde de golpe.

## Color por lado

Como las demás propiedades de borde, acepta de 1 a 4 valores (horario) o propiedades por lado:

```css
border-color: red blue;             /* vertical · horizontal */
border-color: red green blue gold;  /* T R B L */
border-top-color: crimson;          /* solo arriba */
```

Esto permite bordes multicolor, aunque es poco común.

## Transparencia y trucos

Un borde con color **transparente** (`border-color: transparent`) reserva el espacio del borde sin pintarlo, útil para:
- Evitar el "salto" al añadir un borde en `:hover` (se reserva con transparente y se colorea al hacer hover).
- Crear triángulos CSS (la técnica clásica con bordes transparentes).

```css
.boton {
  border: 2px solid transparent;   /* reserva espacio, sin verse */
}
.boton:hover { border-color: currentColor; }   /* aparece sin mover el layout */
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Aprovecha `currentColor` (el valor por defecto) para que el borde siga al texto.
> - Usa `border-color: transparent` para reservar espacio y evitar saltos en `:hover`.
> - Define el color en variables para temas consistentes.
> - Verifica el contraste si el borde transmite información (un campo con error).

## Errores comunes

> [!warning] Trampas
> - **Definir color sin estilo**: el borde no se ve (falta `border-style`).
> - **Salto de layout** al añadir un borde en hover: reserva con `transparent`.
> - **Olvidar que el inicial es `currentColor`**, no negro: el borde puede no ser del color esperado.

## Notas relacionadas

- [[01 color]] · [[01 Palabras Clave de Color]] — `currentColor`, el valor por defecto.
- [[04 Shorthand border]] — definir color junto con grosor y estilo.
- [[05 Colores/index]] — los formatos de color que acepta.
