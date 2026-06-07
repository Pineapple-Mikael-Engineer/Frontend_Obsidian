---
title: font-kerning — Ajuste de espacio entre pares de letras
aliases:
  - font-kerning
  - kerning
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: font-kerning
grupo: tipografia
valor_inicial: auto
hereda: true
animable: false
draft: false
---

# font-kerning

> [!definicion]
> `font-kerning` controla el **kerning**: el ajuste fino del espacio entre **pares concretos** de letras (como "AV" o "To") que la fuente define para que se vean mejor juntas. No es el espaciado general entre letras (eso es [[12 letter-spacing y word-spacing | `letter-spacing`]]), sino el ajuste par a par diseñado por el tipógrafo.

```css
h1 { font-kerning: normal; }
```

## Valores

| Valor | Efecto |
|-------|--------|
| `auto` | El navegador decide (suele activar kerning en texto grande) |
| `normal` | Aplica el kerning de la fuente |
| `none` | Desactiva el kerning |

## Qué es el kerning

> [!info] El espacio diseñado entre pares
> Algunas combinaciones de letras dejan huecos visualmente irregulares sin ajuste: la "A" y la "V" ("AV") tienen formas que crean un espacio aparente mayor. El **kerning** son reglas que la fuente incluye para **acercar o separar** esos pares concretos, logrando un espaciado óptico uniforme. Es distinto del `letter-spacing`, que separa **todas** las letras por igual.

```
Sin kerning:  A V T o   (huecos irregulares)
Con kerning:  AV To     (espaciado uniforme)
```

## auto vs. normal

> [!info] Los navegadores ya lo activan en texto grande
> Con `auto`, los navegadores suelen **activar** el kerning automáticamente en texto de cierto tamaño (donde se nota) y a veces lo desactivan en texto muy pequeño (por rendimiento). `normal` lo fuerza siempre. En la práctica, el kerning ya está activo por defecto en la mayoría de los casos importantes (títulos), así que rara vez hay que tocarlo.

## Cuándo desactivarlo (none)

`font-kerning: none` es útil en casos muy específicos donde el kerning estorba: tablas de caracteres monoespaciados, alineaciones precisas, o cuando se quiere un espaciado perfectamente uniforme controlado solo con `letter-spacing`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Normalmente no hace falta tocarlo: `auto` ya activa el kerning donde importa.
> - `normal` si quieres garantizar el kerning también en texto pequeño.
> - `none` solo en casos de alineación monoespaciada precisa.
> - No confundas kerning (pares) con `letter-spacing` (todas las letras).

## Errores comunes

> [!warning] Trampas
> - **Confundirlo con `letter-spacing`**: uno ajusta pares, el otro separa todo.
> - **Esperar un gran cambio**: el kerning es sutil, se nota sobre todo en títulos.
> - **Desactivarlo sin motivo**: empeora la calidad tipográfica.

## Notas relacionadas

- [[12 letter-spacing y word-spacing]] — el espaciado general, distinto del kerning.
- [[02 font-feature-settings]] — control de bajo nivel de features OpenType.
- [[06 text-rendering]] — `optimizeLegibility` también afecta al kerning.
