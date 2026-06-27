---
title: float — Flotar un elemento
aliases:
  - float
tags:
  - css
  - api/propiedad
  - layout
propiedad: float
grupo: layout
valor_inicial: none
hereda: false
animable: false
draft: false
order: 1
---

# float

> [!definicion]
> `float` saca un elemento del flujo normal y lo desplaza a la **izquierda** o la **derecha** de su contenedor, permitiendo que el **contenido siguiente lo rodee**. Su uso vigente es envolver texto alrededor de una imagen; para layout, se usan Flexbox/Grid.

```css
.foto { float: left; margin: 0 1rem 1rem 0; }
```

## Valores

| Valor | Efecto |
|-------|--------|
| `none` | Sin flotar (por defecto) |
| `left` | Flota a la izquierda; el contenido lo rodea por la derecha |
| `right` | Flota a la derecha; el contenido lo rodea por la izquierda |
| `inline-start` / `inline-end` | Versiones lógicas (RTL) |

## El uso correcto: envolver texto

> [!tip] El texto fluye alrededor de la imagen
> El comportamiento original y útil: una imagen flotada deja que el texto del párrafo la **rodee**, como en una revista o periódico:
> ```css
> .articulo img {
>   float: left;
>   margin: 0 1rem 0.5rem 0;   /* aire entre la imagen y el texto que la rodea */
>   max-width: 40%;
> }
> ```
> El `margin` separa la imagen del texto. Esto es algo que **ni Flexbox ni Grid pueden hacer** (el texto fluyendo alrededor), así que el float sigue siendo la herramienta para este caso.

## Lo que le pasa al float

> [!info] Sale del flujo, pero el texto lo "ve"
> Un float es peculiar: **sale del flujo** (los bloques siguientes ocupan su espacio como si no estuviera), pero el **texto en línea** sí lo respeta y lo rodea. Esta semidesconexión es la fuente de los problemas históricos: el contenedor de un float puede **colapsar** (no calcular su altura), porque "no ve" el float. De ahí el [[03 Clearfix | clearfix]].

## Por qué no maquetar con float

> [!warning] Float para layout = frágil
> Maquetar columnas con `float` requiere: anchos calculados a mano, [[02 clear | `clear`]] para separar filas, el [[03 Clearfix | hack del clearfix]] para que el contenedor no colapse, y se rompe con facilidad. Todo eso lo resuelven Flexbox/Grid de forma limpia. **Regla**: `float` solo para envolver texto; nunca para columnas en código nuevo.

## shape-outside: refinar el contorno

Una mejora moderna: `shape-outside` hace que el texto rodee el float siguiendo una **forma** (un círculo, un polígono, el contorno de una imagen con transparencia) en vez del rectángulo:

```css
.foto-redonda {
  float: left;
  shape-outside: circle(50%);   /* el texto rodea el círculo, no la caja */
  clip-path: circle(50%);
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `float` **solo** para envolver texto alrededor de una imagen.
> - Añade `margin` para separar el float del texto que lo rodea.
> - Para layout (columnas, rejillas), usa Flexbox/Grid, nunca floats.
> - `shape-outside` para que el texto rodee una forma, no solo un rectángulo.

## Errores comunes

> [!warning] Trampas
> - **Maquetar columnas con float**: frágil y obsoleto.
> - **El contenedor colapsa** sin clearfix (no contiene la altura del float).
> - **Olvidar el `margin`**: el texto queda pegado a la imagen.

## Notas relacionadas

- [[02 clear]] — impedir que un elemento se ponga junto a un float.
- [[03 Clearfix]] — que el contenedor contenga sus floats.
- [[03 Flexbox/index]] — la herramienta correcta para layout.
