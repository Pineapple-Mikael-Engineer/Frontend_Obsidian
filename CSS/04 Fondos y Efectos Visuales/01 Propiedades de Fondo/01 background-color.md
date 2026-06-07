---
title: background-color — Color de fondo
aliases:
  - background-color
tags:
  - css
  - api/propiedad
  - fondos
propiedad: background-color
grupo: fondo
valor_inicial: transparent
hereda: false
animable: true
draft: false
---

# background-color

> [!definicion]
> `background-color` pinta el **color de fondo** de una caja, bajo su contenido y padding. Su valor inicial es `transparent` (deja ver lo que hay detrás). Acepta cualquier [[05 Colores/index | formato de color]] y es la capa de fondo más básica.

```css
.aviso { background-color: #fef3c7; }
.boton { background-color: oklch(0.7 0.15 300); }
```

## Es la capa de fondo inferior

> [!info] El color va debajo de las imágenes de fondo
> Cuando un elemento tiene **imagen** y **color** de fondo, el color se pinta **debajo** de la imagen. Esto es útil como **respaldo**: si la imagen no carga (o mientras carga), se ve el color en su lugar, evitando un fondo blanco o transparente feo:
> ```css
> .hero {
>   background-color: #1e1e2e;          /* respaldo mientras carga la foto */
>   background-image: url("foto.jpg");
>   background-size: cover;
> }
> ```

## Ejemplos de uso

```css
/* Tarjeta con fondo sutil */
.tarjeta { background-color: #f8f8fc; }

/* Estado hover de un botón */
.boton:hover { background-color: color-mix(in oklch, var(--btn) 85%, black); }

/* Resaltado de fila al pasar el ratón */
tbody tr:hover { background-color: #f0f0f5; }

/* Overlay semitransparente */
.overlay { background-color: rgb(0 0 0 / 0.5); }
```

## Color y contraste

> [!warning] Declara color junto a background-color
> El `background-color` forma con [[01 color | `color`]] el par cuyo **contraste** determina la legibilidad. Conviene declararlos **juntos**: poner solo el fondo (o solo el texto) puede dejar texto invisible si el otro cambia, sobre todo en modo oscuro o con estilos de usuario:
> ```css
> .chip { background-color: #cba6f7; color: #1e1e2e; }  /* par completo */
> ```
> Verifica la ratio mínima (4.5:1 para texto normal).

## Transparencia y semitransparencia

`transparent` (valor inicial) deja ver lo de detrás. Un color con alfa (`rgb(0 0 0 / 0.5)`) crea **overlays** y capas semitransparentes, muy usados para oscurecer una imagen bajo un texto:

```css
.tinte-oscuro { background-color: rgb(30 30 46 / 0.6); }
```

## Animar el color

`background-color` es **animable**, base de transiciones de hover suaves:

```css
.boton { transition: background-color 0.2s; }
.boton:hover { background-color: #b48ef0; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Declara `background-color` junto a `color` para garantizar contraste.
> - Úsalo como **respaldo** bajo imágenes de fondo.
> - Define los colores en [[10 Variables CSS/index | variables]] para temas consistentes.
> - Para overlays, usa un color con alfa (`rgb(... / 0.5)`).
> - Anímalo en transiciones de hover para feedback suave.

## Errores comunes

> [!warning] Trampas
> - **Solo `background-color` o solo `color`**: riesgo de texto ilegible al cambiar el otro.
> - **Contraste insuficiente**: fondo y texto demasiado parecidos.
> - **Sin color de respaldo** bajo una imagen que puede tardar/fallar.

## Notas relacionadas

- [[01 color]] — el texto, su par de contraste.
- [[02 background-image]] — la imagen que se pinta sobre el color.
- [[08 Shorthand background]] — declarar color y demás de una vez.
