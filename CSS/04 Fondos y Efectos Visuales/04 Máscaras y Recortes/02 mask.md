---
title: mask — Enmascarar con imágenes y gradientes
aliases:
  - mask
  - css mask
tags:
  - css
  - api/propiedad
  - fondos
propiedad: mask
grupo: fondo
valor_inicial: none
hereda: false
animable: false
draft: false
---

# mask

> [!definicion]
> `mask` usa una **imagen o gradiente** para controlar la **opacidad** de cada zona de un elemento: donde la máscara es opaca, el elemento se ve; donde es transparente, desaparece; en zonas intermedias, se desvanece **gradualmente**. A diferencia de [[01 clip-path | `clip-path`]] (bordes nítidos), `mask` permite transiciones suaves.

```css
.fade-bottom {
  mask: linear-gradient(to bottom, black 70%, transparent);
}
```

## Cómo funciona la máscara

> [!info] La luminosidad/alfa define la visibilidad
> El elemento se muestra según la **opacidad de la máscara** en cada punto:
> - Donde la máscara es **opaca/blanca** → el elemento es **visible**.
> - Donde es **transparente** → el elemento es **invisible**.
> - En **gradientes** → transición gradual (desvanecido).
>
> Por eso un gradiente de negro a transparente como máscara crea un **fundido** del elemento.

## Recetas comunes

```css
/* Desvanecer el borde inferior de una imagen (fade out) */
.foto-fade {
  mask: linear-gradient(to bottom, black 60%, transparent);
}

/* Desvanecer ambos lados de un carrusel (scroll horizontal) */
.carrusel {
  mask: linear-gradient(to right, transparent, black 2rem, black calc(100% - 2rem), transparent);
}

/* Recortar con una forma de imagen (un icono como máscara) */
.icono-color {
  background: #cba6f7;
  mask: url("estrella.svg") center / contain no-repeat;
  /* el fondo morado toma la forma de la estrella */
}
```

## El truco de recolorear iconos

> [!tip] Iconos monocromos de cualquier color
> Un uso muy práctico: usar un SVG monocromo como **máscara** sobre un fondo de color permite **recolorear** el icono con CSS (con `currentColor` o variables), algo imposible con `<img>`:
> ```css
> .icono {
>   background-color: currentColor;       /* el color del icono = color del texto */
>   mask: url("icono.svg") center / contain no-repeat;
>   width: 1.5rem; aspect-ratio: 1;
> }
> ```
> El icono toma el color del fondo, que sigue al texto. Cambiar `color` recolorea el icono.

## Las sub-propiedades

`mask` es un shorthand análogo a `background`:

| Sub-propiedad | Análoga a |
|---------------|-----------|
| `mask-image` | `background-image` |
| `mask-position` | `background-position` |
| `mask-size` | `background-size` |
| `mask-repeat` | `background-repeat` |
| `mask-mode` | (luminancia o alfa) |
| `mask-composite` | Combinar varias máscaras |

## Prefijos y soporte

> [!warning] Aún puede necesitar -webkit-
> Aunque el soporte de `mask` ha mejorado mucho, en algunos navegadores aún conviene el prefijo `-webkit-mask` como respaldo:
> ```css
> -webkit-mask: linear-gradient(black, transparent);
> mask: linear-gradient(black, transparent);
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `mask` para desvanecidos graduales (bordes que se difuminan) y recoloreo de iconos.
> - Para formas geométricas nítidas, `clip-path` es más simple.
> - Para iconos monocromos recolorables, `mask` + `background-color: currentColor`.
> - Añade el prefijo `-webkit-mask` como respaldo si soportas navegadores antiguos.

## Errores comunes

> [!warning] Trampas
> - **Esperar bordes nítidos** de un gradiente de máscara: son graduales (usa `clip-path` para nítidos).
> - **Olvidar `-webkit-mask`** en navegadores que lo requieren.
> - **Máscara invertida**: recuerda que lo **opaco** muestra y lo **transparente** oculta.

## Notas relacionadas

- [[01 clip-path]] — recorte con bordes nítidos.
- [[03 Modos de Mezcla (mix-blend-mode, background-blend-mode)]] — combinar capas de otra forma.
- [[02 Gráficos Vectoriales (svg)]] — los SVG usados como máscara.
