---
title: align-items y align-self — Alinear en el eje transversal
aliases:
  - align-items
  - align-self
tags:
  - css
  - api/propiedad
  - layout
propiedad: align-items
grupo: flexbox
valor_inicial: stretch
hereda: false
animable: false
draft: false
order: 5
---

# align-items y align-self

> [!definicion]
> `align-items` alinea **todos** los items en el **eje transversal** (perpendicular al principal): arriba/abajo en una fila, izquierda/derecha en una columna. `align-self` hace lo mismo para **un item concreto**, anulando el `align-items` del contenedor. Por defecto los items se **estiran** para llenar el contenedor.

```css
.barra { display: flex; align-items: center; }   /* todos centrados verticalmente */
.barra .alto { align-self: flex-start; }          /* este, arriba */
```

## Valores

| Valor | Alineación en el eje transversal |
|-------|----------------------------------|
| `stretch` | Estira los items para llenar (por defecto) |
| `flex-start` | Al inicio del eje transversal |
| `flex-end` | Al final |
| `center` | Centrados |
| `baseline` | Alinea por la **línea base** del texto de los items |

## stretch: el valor por defecto que sorprende

> [!info] Por defecto, los items se estiran
> El valor inicial es `stretch`: en una fila, todos los items toman la **misma altura** (la del más alto, o la del contenedor), estirándose. Esto es útil (columnas de igual altura gratis) pero sorprende cuando no lo esperas:
> ```css
> /* Sin tocar align-items: todas las tarjetas tienen la misma altura */
> .fila { display: flex; }
> ```
> Si quieres que cada item tome su altura natural, usa `align-items: flex-start`.

## center: centrar verticalmente

El uso más común en una fila: centrar los items verticalmente, esencial para alinear texto con iconos o botones de distinta altura:

```css
.con-icono { display: flex; align-items: center; gap: 0.5rem; }
/* el icono y el texto quedan centrados entre sí */
```

## align-self: la excepción por item

`align-self` permite que **un** item se alinee distinto del resto, sin tocar el contenedor:

```css
.barra { display: flex; align-items: center; }
.barra .destacado { align-self: stretch; }   /* este sí se estira */
```

## baseline: alinear por el texto

> [!tip] baseline para textos de distinto tamaño
> `align-items: baseline` alinea los items por la **línea base** de su texto, no por sus cajas. Útil cuando hay textos de distinto tamaño que deben "sentarse" sobre la misma línea (un precio grande junto a "/mes" pequeño):
> ```css
> .precio { display: flex; align-items: baseline; gap: 0.25rem; }
> ```

## El eje depende de flex-direction

> [!warning] align-items actúa en el eje TRANSVERSAL
> Como [[04 Eje Principal (justify-content) | `justify-content`]], su eje cambia con [[02 Dirección (flex-direction) | `flex-direction`]]: en `row` alinea **vertical**, en `column` alinea **horizontal**. El intercambio de ejes es la fuente de confusión número uno de Flexbox.

## Buenas prácticas

> [!tip] Recomendaciones
> - `align-items: center` para alinear texto con iconos/botones de distinta altura.
> - Recuerda que el valor por defecto `stretch` iguala las alturas (útil, pero sorprende).
> - `align-self` para que un item rompa la alineación general.
> - `baseline` cuando hay textos de distinto tamaño que deben sentarse en la misma línea.

## Errores comunes

> [!warning] Trampas
> - **Sorpresa por `stretch`**: los items toman la misma altura sin pedirlo.
> - **Confundir el eje**: en `column`, `align-items` es horizontal.
> - **Usar `align-items` para un solo item**: usa `align-self`.

## Notas relacionadas

- [[04 Eje Principal (justify-content)]] — el eje principal, su complemento.
- [[06 Líneas Múltiples (align-content)]] — alinear líneas (con wrap), distinto de align-items.
- [[02 Dirección (flex-direction)]] — define el eje transversal.
