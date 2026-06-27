---
title: Shorthand border — Grosor, estilo y color de una vez
aliases:
  - border shorthand
tags:
  - css
  - api/propiedad
  - layout
propiedad: border
grupo: box-model
valor_inicial: "ver sub-propiedades"
hereda: false
animable: true
draft: false
order: 4
---

# Shorthand border

> [!definicion]
> `border` es el **atajo** que define grosor, estilo y color del borde en una sola declaración. Es la forma habitual de poner un borde, porque obliga a incluir el `border-style` (sin el cual el borde no se vería).

```css
.caja { border: 2px solid #cba6f7; }
/*              grosor estilo color */
```

## Orden de los valores

El orden recomendado es **grosor · estilo · color**, aunque el navegador es flexible (el estilo es el único obligatorio):

```css
border: 1px solid black;
border: solid;              /* solo estilo: grosor medium, color currentColor */
border: 2px dashed;         /* grosor + estilo, color currentColor */
```

> [!info] Solo el estilo es imprescindible
> De los tres, **`border-style` es el único que debe estar** para que el borde se vea. El grosor toma `medium` y el color `currentColor` si se omiten. Por eso `border: solid` ya muestra un borde.

## El shorthand resetea lo no incluido

> [!warning] border sobrescribe las tres sub-propiedades
> Como todo shorthand, `border` **resetea** las tres propiedades, incluso las que no mencionas (a su valor inicial). Si antes pusiste `border-color: red` y luego `border: 2px solid`, el color vuelve a `currentColor`:
> ```css
> .x { border-color: red; }
> .x { border: 2px solid; }   /* el color rojo se PIERDE → currentColor */
> ```
> Si quieres cambiar solo un aspecto sin tocar los otros, usa la sub-propiedad concreta (`border-width`, `border-color`…), no el shorthand.

## border por lado

Existe un shorthand por cada lado, que define los tres aspectos de ese borde:

```css
border-top: 1px solid #ccc;
border-bottom: 3px solid #cba6f7;
border-inline-start: 4px solid;    /* lógico (izq. en LTR) */
```

Muy usado para subrayados de borde, pestañas y citas con barra lateral.

## Quitar un borde

```css
border: none;    /* o border: 0 — elimina el borde */
border-top: none;  /* quita solo el de arriba */
```

## Recetas comunes

```css
/* Tarjeta con borde sutil */
.tarjeta { border: 1px solid #313244; border-radius: 0.5rem; }

/* Cita con barra lateral */
blockquote { border-inline-start: 4px solid #cba6f7; padding-inline-start: 1rem; }

/* Botón fantasma que rellena en hover */
.btn-ghost { border: 2px solid currentColor; background: transparent; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa el shorthand `border` para definir un borde completo: garantiza que incluyes el estilo.
> - Para cambiar **un** aspecto sin tocar los otros, usa la sub-propiedad, no el shorthand.
> - `border-<lado>` para bordes parciales (pestañas, citas).
> - Combina con [[05 border-radius | `border-radius`]] para esquinas redondeadas.

## Errores comunes

> [!warning] Trampas
> - **El shorthand resetea** las propiedades no incluidas (pierdes un color puesto antes).
> - **Olvidar el estilo** si usas las sub-propiedades sueltas en vez del shorthand.
> - **Salto de layout** al añadir el borde: compénsalo con `border-color: transparent` previo.

## Notas relacionadas

- [[01 border-width]] · [[02 border-style]] · [[03 border-color]] — las sub-propiedades.
- [[05 border-radius]] — redondear las esquinas.
- [[01 outline]] — alternativa que no ocupa espacio.
