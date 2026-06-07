---
title: text-emphasis — Marcas de énfasis sobre los caracteres
aliases:
  - text-emphasis
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: text-emphasis
grupo: tipografia
valor_inicial: none
hereda: true
animable: false
draft: false
---

# text-emphasis

> [!definicion]
> `text-emphasis` aplica **marcas de énfasis** (puntos, círculos, sésamos) sobre o bajo **cada carácter**, una convención tipográfica del este de Asia (japonés, chino) equivalente a la cursiva o la negrita occidental para destacar texto. Es de uso de nicho en sitios occidentales pero esencial en CJK.

```css
.enfasis {
  text-emphasis: filled dot;
  text-emphasis-color: crimson;
}
```

## Sintaxis

`text-emphasis` es un shorthand de la marca y su color:

| Sub-propiedad | Controla |
|---------------|----------|
| `text-emphasis-style` | La forma de la marca |
| `text-emphasis-color` | Su color |
| `text-emphasis-position` | Sobre o bajo el texto |

## Las formas

| Valor de estilo | Marca |
|-----------------|-------|
| `dot` | Punto pequeño |
| `circle` | Círculo |
| `double-circle` | Doble círculo |
| `triangle` | Triángulo |
| `sesame` | "Sésamo" (forma tradicional japonesa) |
| `filled` / `open` | Relleno / hueco (modifica las anteriores) |
| `"x"` | Un carácter personalizado como marca |

```css
text-emphasis: filled sesame;   /* sésamo relleno, la marca japonesa clásica */
text-emphasis: open circle;     /* círculo hueco */
```

## La posición

`text-emphasis-position` decide si las marcas van **encima** o **debajo**, importante porque en escritura vertical y según el idioma la convención cambia:

```css
text-emphasis-position: over right;   /* encima (horizontal) */
```

## Por qué se usa en CJK

> [!info] El "bold/italic" del japonés y el chino
> En japonés y chino, la negrita y la cursiva no funcionan bien para enfatizar (los ideogramas no tienen cursiva real, y la negrita los emborrona). La convención es poner una **marca de énfasis** (un punto o sésamo) sobre cada carácter del fragmento a destacar. `text-emphasis` reproduce esto de forma nativa, sin trucos. En sitios occidentales es raro, pero conocerlo es clave para tipografía CJK.

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsalo para enfatizar texto en japonés/chino (su uso natural).
> - `text-emphasis-position` adecuada al idioma y modo de escritura.
> - En sitios occidentales, es un efecto decorativo poco común: valora si aporta.
> - Combínalo con [[01 writing-mode | `writing-mode`]] vertical para CJK auténtico.

## Errores comunes

> [!warning] Trampas
> - **Posición inadecuada** para el modo de escritura.
> - **Abusar en texto occidental**: la cursiva/negrita es la convención allí.
> - **Marca demasiado grande** que satura el texto.

## Notas relacionadas

- [[01 writing-mode]] — la escritura vertical, contexto natural de CJK.
- [[01 text-shadow]] — otro efecto de texto.
- [[05 Énfasis (em)]] — el énfasis semántico desde HTML.
