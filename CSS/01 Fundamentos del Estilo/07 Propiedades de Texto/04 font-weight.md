---
title: font-weight — Grosor del texto
aliases:
  - font-weight
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: font-weight
grupo: tipografia
valor_inicial: normal
hereda: true
animable: true
draft: false
---

# font-weight

> [!definicion]
> `font-weight` define el **grosor** (peso) del texto, desde fino hasta negro. Acepta palabras clave (`normal`, `bold`) o valores numéricos de 100 a 900. El grosor disponible depende de los pesos que la **fuente** incluya.

```css
h1     { font-weight: 700; }     /* negrita */
.fina  { font-weight: 300; }     /* light */
strong { font-weight: bold; }
```

## Valores

| Valor numérico | Nombre | Equivale a |
|----------------|--------|-----------|
| 100 | Thin | — |
| 200 | Extra Light | — |
| 300 | Light | — |
| 400 | Normal / Regular | `normal` |
| 500 | Medium | — |
| 600 | Semi Bold | — |
| 700 | Bold | `bold` |
| 800 | Extra Bold | — |
| 900 | Black | — |

| Palabra clave | Significa |
|---------------|-----------|
| `normal` | 400 |
| `bold` | 700 |
| `lighter` | Un grado más fino que el padre |
| `bolder` | Un grado más grueso que el padre |

## El peso depende de la fuente

> [!warning] No todas las fuentes tienen todos los pesos
> Pedir `font-weight: 300` no garantiza un texto fino: si la fuente cargada **no incluye** ese peso, el navegador usa el más cercano disponible o **sintetiza** uno (lo engorda/adelgaza artificialmente), con peor calidad. Antes de usar pesos intermedios (300, 500, 600), asegúrate de **cargar** esas variantes de la fuente con [[01 Fuentes Web (@font-face)/index | `@font-face`]]:
> ```css
> /* cargar 400 y 700; pedir 600 sin cargarlo da un resultado sintético */
> ```

## Fuentes variables: cualquier peso

> [!tip] Variable fonts permiten pesos exactos
> Una **fuente variable** contiene un eje de peso continuo, lo que permite **cualquier** valor (incluso `font-weight: 437`) con un solo archivo, sin cargar una variante por peso:
> ```css
> h1 { font-weight: 625; }   /* posible con una fuente variable */
> ```
> Es más eficiente (un archivo) y flexible. Detalle en [[03 font-variation-settings]].

## strong y b no fijan el peso

El navegador pone `<strong>` y `<b>` en `font-weight: bold` (700) por defecto, pero es solo un estilo de UA: se puede cambiar con CSS. La semántica de [[04 Énfasis Fuerte (strong) | `<strong>`]] (importancia) es independiente de su grosor visual.

## Animación

`font-weight` es animable **entre valores numéricos** (sobre todo con fuentes variables, que tienen pesos intermedios reales). Con fuentes estáticas, el salto entre 400 y 700 es brusco.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa valores numéricos para precisión; carga los pesos que uses.
> - Considera una **fuente variable** si necesitas varios pesos: un archivo, cualquier valor.
> - No abuses de pesos muy finos (100–200) en texto pequeño: pierde legibilidad.
> - Recuerda que el grosor real depende de lo que la fuente ofrezca.

## Errores comunes

> [!warning] Trampas
> - **Pedir un peso no cargado**: el navegador lo sintetiza con mala calidad.
> - **Texto fino sobre fondo claro**: bajo contraste y poca legibilidad.
> - **Cargar 9 pesos** que no usas: peso de descarga innecesario.

## Notas relacionadas

- [[03 font-variation-settings]] — pesos continuos con fuentes variables.
- [[01 Fuentes Web (@font-face)/index]] — cargar las variantes de peso.
- [[05 font-style]] — cursiva, la otra dimensión del estilo de fuente.
