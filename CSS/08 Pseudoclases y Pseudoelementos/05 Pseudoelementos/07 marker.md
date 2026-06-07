---
title: marker — El marcador de las listas
aliases:
  - "::marker"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: "::marker"
grupo: pseudoelemento
draft: false
---

# ::marker

> [!definicion]
> `::marker` estiliza el **marcador** de los elementos de lista: la viñeta (•) de las listas no ordenadas o el número de las ordenadas. Permite cambiar su color, tamaño y contenido sin recurrir a los antiguos trucos con `::before`.

```css
li::marker { color: #cba6f7; font-weight: bold; }
```

## Estilar viñetas y números

> [!tip] La forma moderna de personalizar marcadores
> Antes de `::marker`, cambiar el color o tamaño de una viñeta requería quitarla (`list-style: none`) y recrearla con [[01 before y after | `::before`]]. `::marker` lo hace directamente:
> ```css
> ul li::marker {
>   color: #cba6f7;
>   font-size: 1.2em;
> }
> ol li::marker {
>   color: #cba6f7;
>   font-weight: bold;
> }
> ```

## Cambiar el contenido del marcador

> [!tip] Marcadores personalizados con content
> `::marker` acepta la propiedad [[02 Propiedad content | `content`]] para reemplazar la viñeta/número por otra cosa:
> ```css
> li::marker { content: "→ "; }          /* flechas en vez de viñetas */
> li::marker { content: "✓ "; color: green; }
>
> /* Números con formato en listas ordenadas */
> ol li::marker { content: counter(list-item) ") "; }
> ```

## Propiedades limitadas

> [!warning] Solo un subconjunto de propiedades
> `::marker` acepta un conjunto **acotado**: `color`, `font-*`, `content`, `white-space`, `text-transform`, `animation`/`transition` de esas propiedades. **No** acepta `background`, márgenes, padding ni la mayoría de propiedades de caja. Para personalizar más allá de eso (un fondo, una forma compleja), aún hace falta el truco de `list-style: none` + `::before`.

## marker vs. list-style

> [!info] Cuándo cada uno
> Para personalizar marcadores hay tres niveles:
> - **`list-style-type`**: cambiar el **tipo** de marcador (disc, circle, decimal, un emoji): `list-style-type: "★"`.
> - **`::marker`**: cambiar el **estilo** del marcador (color, tamaño, contenido).
> - **`list-style: none` + `::before`**: control **total** (fondos, posición, formas) cuando `::marker` no basta.
>
> Para la mayoría de casos (color, tamaño, un símbolo), `list-style-type` y `::marker` bastan; el truco de `::before` queda para personalizaciones complejas.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `::marker` para cambiar color, tamaño o contenido de viñetas/números.
> - Es más limpio que el viejo truco de `list-style: none` + `::before`.
> - Para fondos/formas complejas en el marcador, aún necesitas `::before`.
> - Combínalo con `list-style-type` para elegir el símbolo base.

## Errores comunes

> [!warning] Trampas
> - **Esperar `background`/padding** en `::marker`: no los acepta.
> - **Usar `::before`** para cosas que `::marker` ya resuelve (color, tamaño).
> - **Olvidar que `content`** en `::marker` reemplaza la viñeta entera.

## Notas relacionadas

- [[01 before y after]] — el truco antiguo para marcadores complejos.
- [[02 Propiedad content]] — personalizar el contenido del marcador.
- [[04 Listas/index]] — `list-style` y los tipos de marcador.
