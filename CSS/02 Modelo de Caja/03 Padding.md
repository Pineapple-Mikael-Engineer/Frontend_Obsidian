---
title: padding — Relleno interior de la caja
aliases:
  - padding
tags:
  - css
  - api/propiedad
  - layout
propiedad: padding
grupo: box-model
valor_inicial: "0"
hereda: false
animable: true
draft: false
order: 2
---

# Padding

> [!definicion]
> `padding` es el **relleno interior**: el espacio entre el contenido de un elemento y su borde. Recibe el color de fondo y forma parte del área clicable. Es lo que da "aire" dentro de un botón, una tarjeta o un contenedor.

```css
.boton { padding: 0.5rem 1rem; }   /* 0.5rem arriba/abajo, 1rem izq/der */
.tarjeta { padding: 1.5rem; }      /* igual en los cuatro lados */
```

## Las formas del shorthand

`padding` acepta de 1 a 4 valores, en sentido **horario** desde arriba:

| Valores | Interpretación |
|---------|----------------|
| `padding: 1rem` | Los cuatro lados iguales |
| `padding: 1rem 2rem` | Vertical (arriba/abajo) · Horizontal (izq/der) |
| `padding: 1rem 2rem 3rem` | Arriba · Horizontal · Abajo |
| `padding: 1rem 2rem 3rem 4rem` | Arriba · Derecha · Abajo · Izquierda (horario) |

> [!tip] El truco para recordar los 4 valores
> Cuatro valores van en sentido **horario** empezando arriba: **T**op, **R**ight, **B**ottom, **L**eft ("TRouBLe"). Con dos valores, el primero es vertical y el segundo horizontal.

## Las propiedades individuales

| Propiedad | Lado |
|-----------|------|
| `padding-top` / `padding-bottom` | Arriba / abajo |
| `padding-left` / `padding-right` | Izquierda / derecha |
| `padding-block` | Vertical (lógico): `padding-block: 1rem` |
| `padding-inline` | Horizontal (lógico): `padding-inline: 2rem` |

> [!tip] padding-inline y padding-block son muy cómodos
> Las propiedades lógicas `padding-inline` (izq+der) y `padding-block` (arriba+abajo) permiten fijar un eje sin tocar el otro, y se adaptan a RTL:
> ```css
> .seccion { padding-block: 3rem; padding-inline: 1.5rem; }
> ```

## Recibe el fondo y los clics

> [!info] El padding es parte del elemento
> A diferencia del [[05 Margin | margin]], el padding está **dentro** del borde, así que:
> - **Se pinta** del color de fondo (`background`).
> - **Recibe eventos** (hover, clic): amplía el área interactiva.
>
> Por eso, para hacer un botón o enlace con una zona de toque más grande, se usa **padding**, no margin. Un buen objetivo táctil mide al menos ~44px, lo que se logra con padding.

## El padding en % se calcula sobre el ancho

> [!warning] padding vertical en % usa el ancho
> Un detalle sorprendente: `padding-top`/`padding-bottom` en **porcentaje** se calculan sobre el **ancho** del contenedor, no el alto (igual que en [[05 Porcentajes | margin]]). Esto era la base del viejo truco de proporciones (`padding-top: 56.25%`), hoy reemplazado por [[03 aspect-ratio | `aspect-ratio`]].

## Interacción con box-sizing

Con `content-box` (por defecto), el padding **se suma** al `width`; con [[06 box-sizing | `border-box`]], se **resta del interior**, manteniendo el tamaño total. Esto cambia cuánto ocupa realmente una caja con padding, y es la razón principal para usar `border-box`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa padding para el espacio **interior** y para agrandar el área clicable.
> - `padding-inline`/`padding-block` para controlar un eje y soportar RTL.
> - Define el espaciado en `rem` para que escale con la tipografía.
> - Recuerda que con `border-box` el padding no agranda la caja total.

## Errores comunes

> [!warning] Trampas
> - **Usar margin** para el área clicable (no recibe eventos): usa padding.
> - **Olvidar `box-sizing`**: el padding suma al `width` con `content-box`.
> - **`%` vertical** esperando que use el alto: usa el ancho.

## Notas relacionadas

- [[05 Margin]] — el espacio exterior, su contraparte.
- [[06 box-sizing]] — cómo el padding afecta al tamaño total.
- [[01 Partes del Modelo de Caja]] — padding dentro del modelo completo.
