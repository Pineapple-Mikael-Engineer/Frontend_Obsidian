---
title: align-content — Alinear las líneas cuando hay wrap
aliases:
  - align-content
tags:
  - css
  - api/propiedad
  - layout
propiedad: align-content
grupo: flexbox
valor_inicial: stretch
hereda: false
animable: false
draft: false
order: 6
---

# align-content

> [!definicion]
> `align-content` distribuye las **líneas** de un contenedor flex en el eje transversal cuando hay **varias** (es decir, con [[03 Ajuste de Línea (flex-wrap) | `flex-wrap: wrap`]] y espacio sobrante). Es a las líneas lo que [[04 Eje Principal (justify-content) | `justify-content`]] es a los items.

```css
.galeria {
  display: flex;
  flex-wrap: wrap;
  align-content: space-between;
  height: 30rem;
}
```

## Solo funciona con wrap y varias líneas

> [!warning] Sin wrap, align-content no hace nada
> `align-content` requiere **dos condiciones**: que haya `flex-wrap: wrap` y que existan **varias líneas** con espacio sobrante en el eje transversal (un contenedor más alto que el contenido). Si solo hay una línea (nowrap, o el contenido llena), `align-content` se ignora; ahí actúa [[05 Eje Transversal (align-items, align-self) | `align-items`]]:
> ```css
> /* align-content solo importa si hay wrap Y altura sobrante */
> .x { display: flex; flex-wrap: wrap; height: 40rem; align-content: center; }
> ```

## align-content vs. align-items

> [!info] Las líneas vs. los items dentro de cada línea
> La confusión clásica entre las dos propiedades de alineación transversal:
> - **`align-items`**: alinea **cada item dentro de su línea** (arriba/abajo en la línea).
> - **`align-content`**: alinea **las líneas entre sí** dentro del contenedor (cuando hay varias).
>
> Con una sola línea, solo `align-items` importa. Con varias líneas y espacio sobrante, `align-content` reparte las líneas y `align-items` alinea los items dentro de cada una.

## Valores

| Valor | Distribución de las líneas |
|-------|----------------------------|
| `stretch` | Las líneas se estiran para llenar (por defecto) |
| `flex-start` / `flex-end` | Agrupadas al inicio / final |
| `center` | Centradas |
| `space-between` / `space-around` / `space-evenly` | Repartidas con espacio entre ellas |

Son los mismos valores que `justify-content`, aplicados a las líneas.

## Ejemplo

```css
/* Rejilla flex de varias filas, centradas verticalmente en un contenedor alto */
.tarjetas {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  height: 100vh;
  align-content: center;   /* las filas se agrupan en el centro vertical */
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `align-content` solo cuando haya `wrap` y varias líneas con espacio sobrante.
> - No lo confundas con `align-items` (líneas vs. items dentro de la línea).
> - Para rejillas con control de filas y columnas, Grid suele ser más predecible que flex wrap + align-content.
> - El shorthand `place-content` combina `align-content` y `justify-content`.

## Errores comunes

> [!warning] Trampas
> - **Esperar efecto sin wrap** o sin espacio sobrante: se ignora.
> - **Confundirlo con `align-items`**: uno alinea líneas, el otro items.

## Notas relacionadas

- [[05 Eje Transversal (align-items, align-self)]] — alinear items dentro de su línea.
- [[03 Ajuste de Línea (flex-wrap)]] — el requisito (varias líneas).
- [[11 Alineación en Grid (justify, align, place)]] — la alineación equivalente en Grid.
