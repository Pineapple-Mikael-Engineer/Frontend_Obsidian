---
title: Shorthand background — Todas las propiedades de fondo a la vez
aliases:
  - background shorthand
tags:
  - css
  - api/propiedad
  - fondos
propiedad: background
grupo: fondo
valor_inicial: "ver sub-propiedades"
hereda: false
animable: false
draft: false
---

# Shorthand background

> [!definicion]
> `background` es el **atajo** que combina todas las propiedades de fondo (color, imagen, posición, tamaño, repetición, attachment, origin, clip) en una sola declaración. Es la forma compacta y habitual de definir fondos.

```css
.hero {
  background: #1e1e2e url("foto.jpg") center / cover no-repeat;
}
```

## El orden y la barra del tamaño

> [!info] position / size con barra
> El orden es flexible salvo un detalle: el **tamaño** va **después de la posición**, separado por una **barra `/`**:
> ```css
> background: <color> <imagen> <posición> / <tamaño> <repeat> <attachment> <origin/clip>;
> background: #1e1e2e url("f.jpg") center / cover no-repeat fixed;
> /*          color    imagen     posición/tamaño  repeat  attachment */
> ```
> La barra entre posición y tamaño es obligatoria si incluyes el tamaño; sin ella, el navegador no sabe cuál es cuál.

## El shorthand resetea todo

> [!warning] background sobrescribe todas las sub-propiedades
> Como todo shorthand, `background` **resetea** todas las propiedades de fondo a su valor inicial, incluso las que no mencionas. Si antes pusiste `background-repeat: no-repeat` y luego `background: url(...)`, la repetición vuelve a `repeat`:
> ```css
> .x { background-repeat: no-repeat; }
> .x { background: url("f.jpg"); }   /* ❌ no-repeat se PIERDE → repeat */
> ```
> Para cambiar **un** aspecto sin tocar los demás, usa la sub-propiedad concreta (`background-color`, `background-position`…), no el shorthand.

## Recetas comunes

```css
/* Foto de fondo que llena, una vez, centrada, con color de respaldo */
.hero { background: #1e1e2e url("foto.jpg") center / cover no-repeat; }

/* Solo un color (lo más simple) */
.tarjeta { background: #f8f8fc; }

/* Gradiente de fondo */
.banner { background: linear-gradient(135deg, #cba6f7, #89dceb); }

/* Patrón de puntos */
.puntos {
  background: radial-gradient(#cba6f7 2px, transparent 2px) 0 0 / 20px 20px;
}
```

## El color va solo en la última capa

> [!info] En fondos múltiples, el color va al final
> Con [[09 Múltiples Fondos | varias capas]], el `background-color` solo puede ir en la **última** capa de la lista (es la de más abajo). Las capas superiores son imágenes/gradientes; el color es el respaldo del fondo:
> ```css
> background:
>   linear-gradient(...) ,        /* capa superior */
>   url("foto.jpg") center / cover,
>   #1e1e2e;                      /* color, última capa */
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa el shorthand para definir un fondo completo de forma compacta.
> - Recuerda la barra `position / size`.
> - Para cambiar **un** aspecto, usa la sub-propiedad, no el shorthand (que resetea todo).
> - Incluye un `background-color` de respaldo en fondos de imagen.

## Errores comunes

> [!warning] Trampas
> - **El shorthand resetea** propiedades puestas antes (pierdes `no-repeat`, etc.).
> - **Olvidar la `/`** entre posición y tamaño.
> - **Poner el color en una capa que no es la última** en fondos múltiples.

## Notas relacionadas

- [[09 Múltiples Fondos]] — varias capas en el shorthand.
- [[05 background-size]] · [[04 background-position]] — la parte `position / size`.
- [[04 Shorthand border]] — otro shorthand con el mismo comportamiento de reset.
