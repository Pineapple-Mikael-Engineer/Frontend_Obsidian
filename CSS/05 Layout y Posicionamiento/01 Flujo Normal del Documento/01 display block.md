---
title: display block — Elemento de bloque
aliases:
  - display block
tags:
  - css
  - api/propiedad
  - layout
propiedad: display
grupo: layout
valor_inicial: inline
hereda: false
animable: false
draft: false
order: 1
---

# display block

> [!definicion]
> Un elemento con `display: block` empieza en una **línea nueva**, ocupa **todo el ancho** disponible de su contenedor y respeta `width`, `height`, `margin` y `padding` en todos los lados. Es el comportamiento de los contenedores estructurales (`<div>`, `<p>`, `<section>`, `<h1>`–`<h6>`).

```css
.tarjeta { display: block; }   /* (ya es el valor por defecto de un div) */
```

## Características

| Aspecto | Comportamiento |
|---------|----------------|
| Salto de línea | Empieza en línea nueva; el siguiente bloque va debajo |
| Ancho | Ocupa **todo** el ancho disponible (`width: auto`) |
| `width`/`height` | Se respetan |
| `margin`/`padding` | Se respetan en los 4 lados |
| Apilado | Vertical (uno bajo otro) |

## Elementos block por defecto

`<div>`, `<p>`, `<h1>`–`<h6>`, `<section>`, `<article>`, `<header>`, `<footer>`, `<ul>`, `<li>`, `<form>`, `<figure>`… La mayoría de los elementos estructurales y de contenido de bloque.

## Ocupa todo el ancho aunque el contenido sea corto

> [!info] Un bloque llena la línea
> Aunque su contenido sea una sola palabra, un elemento `block` ocupa **todo el ancho** del contenedor (su `width` es `auto`, que en bloque significa "todo lo disponible"). Para que ocupe solo lo que su contenido necesita, hay que cambiar su `display` a `inline-block`/`inline` o usar `width: fit-content`:
> ```css
> .etiqueta { display: block; width: fit-content; }  /* bloque que se ajusta al contenido */
> ```

## Convertir inline en block

`display: block` se usa a menudo para hacer que un elemento normalmente en línea ocupe su propia línea: un enlace que debe ser un botón de bloque, una imagen que no debe dejar el [[03 Alineación Vertical (vertical-align) | hueco fantasma]]:

```css
a.boton-bloque { display: block; }   /* el enlace ocupa toda la línea */
img { display: block; }              /* quita el hueco bajo la imagen */
```

## Buenas prácticas

> [!tip] Recomendaciones
> - `block` es el comportamiento natural de los contenedores; rara vez hay que declararlo.
> - Para que un bloque se ajuste a su contenido, usa `width: fit-content`.
> - `display: block` en una `<img>` elimina el hueco inferior de la línea base.
> - Para colocar bloques **en fila**, no fuerces inline-block: usa [[01 Contenedor Flex (display flex) | Flexbox]].

## Errores comunes

> [!warning] Trampas
> - **Esperar que un bloque se ajuste** a su contenido: ocupa todo el ancho.
> - **Usar muchos `inline-block` en fila** para layout: Flexbox/Grid es mejor.
> - **Confundir `block` con `inline-block`**: el segundo no salta de línea.

## Notas relacionadas

- [[02 display inline]] — el comportamiento opuesto.
- [[03 display inline-block]] — el híbrido.
- [[01 Contenedor Flex (display flex)]] — colocar bloques en fila moderno.
