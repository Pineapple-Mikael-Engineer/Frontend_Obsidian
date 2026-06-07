---
title: font-synthesis — Controlar la negrita y cursiva sintéticas
aliases:
  - font-synthesis
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: font-synthesis
grupo: tipografia
valor_inicial: "weight style small-caps"
hereda: true
animable: false
draft: false
---

# font-synthesis

> [!definicion]
> `font-synthesis` controla si el navegador puede **sintetizar** (falsear) la negrita, la cursiva o las versalitas cuando la fuente **no incluye** esa variante real. Por defecto las sintetiza; esta propiedad permite **desactivarlo** para evitar resultados de baja calidad.

```css
.solo-real { font-synthesis: none; }   /* no falsear nada */
```

## Valores

| Valor | Efecto |
|-------|--------|
| `weight style small-caps` | Permite sintetizar las tres (por defecto) |
| `none` | No sintetiza nada |
| `weight` | Permite solo negrita sintética |
| `style` | Permite solo cursiva sintética |
| `small-caps` | Permite solo versalitas sintéticas |

(Se combinan: `font-synthesis: weight style` permite negrita y cursiva, no versalitas.)

## Qué es la síntesis

> [!info] Negrita y cursiva "falsas"
> Si pides `font-weight: bold` y la fuente cargada **no tiene** una variante Bold, el navegador la **sintetiza**: engorda artificialmente los trazos del Regular. Igual con `font-style: italic` sin variante italic: inclina las letras. El resultado es **inferior** a una variante diseñada: trazos irregulares, formas torpes. `font-synthesis: none` impide ese falseo.

## Cuándo desactivarla

> [!tip] none cuando la calidad tipográfica importa
> `font-synthesis: none` es útil cuando:
> - Cargas una fuente con **pesos/estilos reales** y quieres garantizar que solo se usen esos (si pides un peso no cargado, prefieres que **no** se falsee).
> - Usas una fuente de iconos o muy estilizada donde el falseo la rompería.
>
> El efecto: si pides una variante que la fuente no tiene, el navegador usa la más cercana real en vez de inventarla.

## La contraparte: cargar las variantes reales

La mejor forma de evitar la síntesis no es solo `font-synthesis: none`, sino **cargar las variantes reales** de la fuente (Bold, Italic) con [[01 src y format | `@font-face`]]. Así `bold` e `italic` usan diseños reales y la síntesis nunca entra en juego. `font-synthesis: none` es la red de seguridad por si falta alguna.

## Buenas prácticas

> [!tip] Recomendaciones
> - Carga las variantes reales (Bold, Italic) de tu fuente; evita depender de la síntesis.
> - `font-synthesis: none` para garantizar que no se falsean variantes faltantes.
> - Considera fuentes variables: cubren todos los pesos sin síntesis ni archivos extra.
> - Revisa el resultado: la negrita/cursiva sintética se nota en texto grande.

## Errores comunes

> [!warning] Trampas
> - **Confiar en la síntesis** en vez de cargar variantes reales: peor calidad.
> - **`font-synthesis: none` sin cargar las variantes**: el texto pierde la negrita/cursiva del todo.
> - **No notar el falseo** porque solo se ve mal en titulares grandes.

## Notas relacionadas

- [[04 font-weight]] — la negrita que puede sintetizarse.
- [[05 font-style]] — la cursiva sintética.
- [[01 src y format]] — cargar las variantes reales para no depender de la síntesis.
