---
title: first-line — La primera línea
aliases:
  - "::first-line"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: "::first-line"
grupo: pseudoelemento
draft: false
order: 4
---

# ::first-line

> [!definicion]
> `::first-line` selecciona la **primera línea visual** de un bloque de texto, para estilarla distinto. Lo notable: la "primera línea" se calcula de forma **dinámica** según el ancho del elemento, así que cambia si la ventana se redimensiona.

```css
p::first-line { font-variant: small-caps; color: #cba6f7; }
```

## La primera línea es dinámica

> [!info] No es un número fijo de palabras
> A diferencia de [[03 first-letter | `::first-letter`]] (siempre una letra), `::first-line` abarca **las palabras que caben en la primera línea renderizada**, lo que depende del **ancho** del elemento. Si el usuario ensancha la ventana, caben más palabras en la primera línea, y `::first-line` las incluye. Es un pseudoelemento que reacciona al layout:
> ```
> Ventana estrecha:  [La primera línea]   ← first-line abarca esto
> Ventana ancha:     [La primera línea es más larga]  ← y ahora esto
> ```

## Usos típicos

```css
/* Entradilla con versalitas en la primera línea */
.articulo > p:first-of-type::first-line {
  font-variant: small-caps;
  letter-spacing: 0.05em;
  color: #555;
}

/* Primera línea destacada de una nota */
.nota::first-line { font-weight: 600; }
```

El efecto editorial: la primera línea con versalitas o en negrita, un recurso tipográfico de revistas.

## Propiedades aplicables limitadas

> [!warning] Solo propiedades de texto/fuente
> Como `::first-letter`, `::first-line` acepta un **subconjunto** de propiedades: `font-*`, `color`, `background`, `text-decoration`, `text-transform`, `letter-spacing`, `word-spacing`, `line-height`, `vertical-align`. No acepta propiedades de caja (márgenes, padding, borde) ni de layout, porque la "línea" no es una caja con geometría propia.

## first-line vs. first-of-type

`::first-line` se aplica al **bloque** indicado. Para que solo afecte al primer párrafo, combínalo con `:first-of-type` como en `::first-letter`:

```css
article p:first-of-type::first-line { font-variant: small-caps; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `::first-line` para entradillas con versalitas o negrita en la primera línea.
> - Recuerda que la primera línea es **dinámica** (cambia con el ancho).
> - Solo acepta propiedades de texto/fuente, no de caja.
> - Combínalo con `:first-of-type` para afectar solo al primer párrafo.

## Errores comunes

> [!warning] Trampas
> - **Esperar márgenes/padding**: solo acepta propiedades de texto.
> - **Asumir un número fijo de palabras**: depende del ancho.
> - **Aplicarlo a todos los párrafos** sin `:first-of-type`.

## Notas relacionadas

- [[03 first-letter]] — la primera letra (paralelo, pero fijo).
- [[02 first-of-type y last-of-type]] — limitar al primer párrafo.
- [[06 Variantes (font-variant)]] — `small-caps` para la primera línea.
