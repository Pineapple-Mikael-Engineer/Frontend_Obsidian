---
title: Alineación en Grid — justify, align, place
aliases:
  - grid alignment
  - place-items
  - place-content
tags:
  - css
  - api/propiedad
  - layout
propiedad: place-items
grupo: grid
valor_inicial: stretch
hereda: false
animable: false
draft: false
order: 11
---

# Alineación en Grid (justify, align, place)

> [!definicion]
> Grid alinea en **dos ejes** con dos familias de propiedades: las `*-items`/`*-self` alinean el **contenido dentro de cada celda**, y las `*-content` alinean **la rejilla entera** dentro del contenedor. `justify-*` actúa en el eje **horizontal** (en línea) y `align-*` en el **vertical** (bloque); las `place-*` combinan ambos.

```css
.rejilla {
  display: grid;
  place-items: center;       /* centra el contenido de cada celda */
  place-content: center;     /* centra toda la rejilla en el contenedor */
}
```

## Los dos niveles de alineación

> [!info] El item en su celda vs. la rejilla en el contenedor
> Dos cosas distintas se pueden alinear:
> 1. **El contenido dentro de cada celda** (`justify-items`, `align-items`): cuando el item es más pequeño que su celda, dónde se coloca dentro.
> 2. **La rejilla entera dentro del contenedor** (`justify-content`, `align-content`): cuando la rejilla es más pequeña que el contenedor, dónde se coloca el conjunto.

## Las propiedades

| Propiedad | Qué alinea | Eje |
|-----------|------------|-----|
| `justify-items` | El contenido en su celda | Horizontal |
| `align-items` | El contenido en su celda | Vertical |
| `justify-content` | La rejilla en el contenedor | Horizontal |
| `align-content` | La rejilla en el contenedor | Vertical |
| `justify-self` / `align-self` | Un item concreto en su celda | — |
| `place-items` / `place-content` / `place-self` | Atajo de ambos ejes | Ambos |

## justify vs. align: en línea vs. bloque

> [!info] La regla mnemotécnica
> A diferencia de Flexbox (donde el eje de `justify`/`align` depende de `flex-direction`), en Grid es **fijo**:
> - **`justify-*`** → eje **en línea** (horizontal, en escritura normal).
> - **`align-*`** → eje **de bloque** (vertical).
>
> "justify = horizontal, align = vertical" funciona como regla en Grid (siempre que la escritura sea horizontal).

## place-items: el centrado mínimo

> [!tip] place-items: center, el centrado más corto de CSS
> `place-items` combina `align-items` y `justify-items`. `place-items: center` centra el contenido de cada celda en ambos ejes:
> ```css
> .centro { display: grid; place-items: center; }
> ```
> Es el centrado perfecto más compacto de todo CSS: una sola declaración. Para centrar un único elemento en una caja, es imbatible.

## Recetas comunes

```css
/* Centrar todo el contenido de cada celda */
.rejilla { display: grid; place-items: center; }

/* Alinear cada item a la derecha de su celda */
.precios { display: grid; justify-items: end; }

/* Un item concreto centrado, el resto por defecto */
.especial { place-self: center; }

/* La rejilla centrada en un contenedor más grande */
.contenedor { display: grid; place-content: center; }
```

## justify-content en Grid: distribuir las columnas

Cuando las columnas no llenan el contenedor (anchos fijos en un contenedor más ancho), `justify-content` distribuye las **pistas** (no los items): `space-between`, `center`, etc., igual que en Flexbox pero aplicado a la rejilla.

## Buenas prácticas

> [!tip] Recomendaciones
> - `place-items: center` para el centrado más corto.
> - `justify-items`/`align-items` para alinear el contenido en las celdas; `*-self` para un item concreto.
> - `*-content` para alinear la rejilla entera cuando no llena el contenedor.
> - Recuerda: en Grid, `justify` = horizontal, `align` = vertical (fijo, no depende de la dirección).

## Errores comunes

> [!warning] Trampas
> - **Confundir `*-items` con `*-content`**: uno alinea en la celda, otro la rejilla.
> - **Esperar el comportamiento de Flexbox**: en Grid los ejes de justify/align son fijos.
> - **Olvidar que el valor por defecto es `stretch`**: los items llenan su celda salvo que digas otra cosa.

## Notas relacionadas

- [[05 Eje Transversal (align-items, align-self)]] — la alineación en Flexbox, comparada.
- [[06 Líneas Múltiples (align-content)]] — `align-content`, también en Flexbox.
- [[01 Contenedor Grid]] — `place-items: center` para centrar.
