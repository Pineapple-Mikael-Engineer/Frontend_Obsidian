---
title: Múltiples Fondos — Apilar varias capas
aliases:
  - multiple backgrounds
tags:
  - css
  - api/concepto
  - fondos
draft: false
order: 9
---

# Múltiples Fondos

> [!definicion]
> Un mismo elemento puede tener **varias capas de fondo** apiladas, declaradas en `background-image` (o el shorthand) separadas por **comas**. Cada capa puede ser una imagen o un gradiente con su propia posición, tamaño y repetición. La **primera** capa de la lista va **encima**.

```css
.hero {
  background:
    linear-gradient(rgb(0 0 0 / 0.5), rgb(0 0 0 / 0.5)),  /* capa superior */
    url("foto.jpg") center / cover no-repeat,              /* capa inferior */
    #1e1e2e;                                                /* color de respaldo */
}
```

## El orden: la primera va arriba

> [!info] La primera capa es la más cercana al usuario
> El orden de apilado es **inverso** a la intuición de "pintor": la **primera** capa de la lista se dibuja **encima** de las siguientes. Por eso un overlay (que debe tapar) va **primero**, y la foto que oscurece va **después**:
> ```css
> background:
>   <overlay encima>,
>   <imagen debajo>,
>   <color al fondo>;
> ```

## El caso estrella: overlay para texto legible

> [!tip] Gradiente semitransparente sobre una foto
> El uso más común de fondos múltiples: poner texto legible sobre una imagen, oscureciéndola con un gradiente uniforme encima:
> ```css
> .hero {
>   background:
>     linear-gradient(rgb(0 0 0 / 0.55), rgb(0 0 0 / 0.55)),  /* tinte oscuro uniforme */
>     url("paisaje.jpg") center / cover;
>   color: white;
> }
> ```
> El gradiente "de un solo color con alfa" actúa como un velo, garantizando contraste para el texto blanco sin importar la foto.

## Cada capa tiene sus propiedades

Las sub-propiedades (`background-position`, `-size`, `-repeat`…) también aceptan **listas** separadas por comas, una por capa:

```css
.compuesto {
  background-image: url("estrella.svg"), url("fondo.png");
  background-position: top right, center;
  background-repeat: no-repeat, repeat;
  background-size: 40px, auto;
}
```

La primera entrada de cada propiedad corresponde a la primera capa, y así sucesivamente.

## El color, solo en la última capa

> [!warning] background-color va al final
> El `background-color` es siempre la capa **inferior** (el respaldo), así que en el shorthand solo puede ir en la **última** posición de la lista. No se puede poner un color en una capa intermedia (para eso se usa un gradiente de color sólido con alfa).

## Recetas de patrones combinados

```css
/* Cuadrícula (líneas horizontales + verticales) */
.cuadricula {
  background-image:
    linear-gradient(#ddd 1px, transparent 1px),
    linear-gradient(90deg, #ddd 1px, transparent 1px);
  background-size: 20px 20px;
}

/* Degradado sobre patrón de puntos */
.compuesto {
  background:
    linear-gradient(135deg, rgb(203 166 247 / 0.3), transparent),
    radial-gradient(#cba6f7 1px, transparent 1px) 0 0 / 16px 16px,
    #fff;
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Recuerda: la **primera** capa va **encima**.
> - Overlay (gradiente con alfa) primero, foto después, color al final.
> - Cada sub-propiedad acepta una lista (una entrada por capa).
> - Combina gradientes para crear patrones (cuadrículas, puntos) sin imágenes.

## Errores comunes

> [!warning] Trampas
> - **Orden invertido**: poner la foto antes que el overlay (la foto tapa el overlay).
> - **Color en capa intermedia**: solo va en la última.
> - **Listas de sub-propiedades descuadradas**: el número de entradas debe corresponder a las capas.

## Notas relacionadas

- [[08 Shorthand background]] — declarar las capas en el shorthand.
- [[02 background-image]] — las imágenes/gradientes de cada capa.
- [[02 Gradientes/index]] — los gradientes que componen patrones y overlays.
