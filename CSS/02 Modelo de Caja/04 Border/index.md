---
title: Border — El borde de la caja
aliases:
  - border
tags:
  - css
  - api/concepto
  - layout
draft: false
---

# Border

> [!definicion]
> El **borde** es la línea que rodea el padding de una caja, entre el relleno y el margen. Se define por tres aspectos —**grosor**, **estilo** y **color**— más el redondeo de esquinas (`border-radius`) y la posibilidad de usar una imagen como borde (`border-image`).

```css
.caja {
  border: 2px solid #cba6f7;   /* grosor estilo color */
  border-radius: 0.5rem;       /* esquinas redondeadas */
}
```

## Las propiedades del borde

| Aspecto | Propiedad | Nota |
|---------|-----------|------|
| Grosor | `border-width` | [[01 border-width]] |
| Estilo | `border-style` | [[02 border-style]] |
| Color | `border-color` | [[03 border-color]] |
| Atajo | `border` | [[04 Shorthand border]] |
| Esquinas | `border-radius` | [[05 border-radius]] |
| Imagen | `border-image` | [[06 border-image]] |

## El estilo es obligatorio para que se vea

> [!warning] Sin border-style no hay borde
> El error nº 1 con bordes: definir grosor y color pero **olvidar el estilo**. `border-style` vale `none` por defecto, así que **sin él el borde no se muestra**, por mucho `border-width` y `border-color` que pongas:
> ```css
> .x { border-width: 2px; border-color: red; }   /* ❌ invisible: falta el estilo */
> .x { border: 2px solid red; }                  /* ✅ */
> ```

## El borde ocupa espacio

El borde se suma al tamaño de la caja (con `content-box`) o se descuenta del interior (con [[06 box-sizing | `border-box`]]). Un borde de `2px` en los cuatro lados añade 4px al ancho total con `content-box`. Por eso aparecer/desaparecer un borde en `:hover` puede causar un "salto" de 1-2px si no se compensa.

## Bordes por lado

Cada lado se puede controlar individualmente:

```css
.tab {
  border-bottom: 3px solid #cba6f7;   /* solo el borde inferior */
}
.cita {
  border-inline-start: 4px solid;      /* lógico: izq. en LTR, der. en RTL */
}
```

## Mapa de la subsección

- [[01 border-width]] — el grosor.
- [[02 border-style]] — el estilo (sólido, punteado, etc.), imprescindible.
- [[03 border-color]] — el color.
- [[04 Shorthand border]] — el atajo `border: grosor estilo color`.
- [[05 border-radius]] — redondear esquinas.
- [[06 border-image]] — usar una imagen como borde.

## Notas relacionadas

- [[04 Shorthand border]] — la forma habitual de declarar bordes.
- [[05 border-radius]] — esquinas redondeadas.
- [[01 outline]] — el contorno, parecido al borde pero que **no** ocupa espacio.
