---
title: <sup> y <sub> — Superíndice y subíndice
aliases:
  - sup
  - sub
  - superscript
  - subscript
tags:
  - html
  - api/elemento
  - semantica
elemento: sup
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
---

# Superíndice y Subíndice (sup, sub)

> [!definicion]
> `<sup>` eleva el texto (superíndice) y `<sub>` lo baja (subíndice). Solo deben usarse cuando la posición **cambia el significado**: notación matemática, química, ordinales, notas al pie. No son para ajustar la posición vertical por estética.

```html
<p>El área es <var>r</var><sup>2</sup> · π.</p>
<p>La fórmula del agua es H<sub>2</sub>O.</p>
```

## Cuándo cada uno

| Elemento | Uso típico | Ejemplo |
|----------|-----------|---------|
| `<sup>` | Exponentes | x<sup>2</sup>, 10<sup>-9</sup> |
| `<sup>` | Ordinales (en algunos idiomas) | 1<sup>er</sup>, 2<sup>nd</sup> |
| `<sup>` | Notas al pie | texto<sup>[1]</sup> |
| `<sub>` | Subíndices químicos | H<sub>2</sub>O, CO<sub>2</sub> |
| `<sub>` | Índices matemáticos | x<sub>i</sub>, a<sub>n</sub> |

## El significado debe depender de la posición

> [!info] El criterio de uso
> `<sup>`/`<sub>` solo son apropiados cuando elevar o bajar el texto **es parte del significado**: `CO2` y `CO₂` no significan lo mismo, ni `x2` y `x²`. Si solo quieres mover texto arriba o abajo por diseño (un símbolo de marca registrada estilizado sin valor semántico, un efecto tipográfico), eso es CSS (`vertical-align`, `font-size`), no estos elementos.

## Ejemplos en contexto

```html
<!-- Matemáticas -->
<p>La ecuación cuadrática: <var>a</var><var>x</var><sup>2</sup> +
   <var>b</var><var>x</var> + <var>c</var> = 0</p>

<!-- Química -->
<p>La fotosíntesis consume CO<sub>2</sub> y produce O<sub>2</sub>.</p>

<!-- Nota al pie -->
<p>El estudio lo confirma<sup><a href="#nota1">1</a></sup>.</p>
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<sup>`/`<sub>` solo cuando la posición cambia el significado (fórmulas, notación, notas).
> - Para notación matemática extensa, considera MathML o KaTeX/MathJax; estos elementos cubren lo sencillo en línea.
> - Para efectos puramente visuales, usa CSS, no estos elementos.

## Errores comunes

> [!warning] sup/sub por estética
> Usar `<sup>` para subir un asterisco decorativo o `<sub>` para bajar texto en un logo confunde presentación con semántica. La regla: si quitar el superíndice/subíndice **no** cambia lo que el texto significa, el ajuste es de CSS.

## Notas relacionadas

- [[19 Variable (var)]] — las variables que suelen acompañar a exponentes e índices.
- [[index]] — el resto del marcado de texto.
