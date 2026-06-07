---
title: column-gap y column-rule — Espacio y línea entre columnas
aliases:
  - column-gap
  - column-rule
tags:
  - css
  - api/propiedad
  - layout
propiedad: column-gap
grupo: layout
valor_inicial: normal
hereda: false
animable: false
draft: false
---

# column-gap y column-rule

> [!definicion]
> `column-gap` define el **espacio** entre las columnas de un layout multicolumna; `column-rule` dibuja una **línea separadora** en ese espacio (como el filete entre columnas de un periódico). La línea no ocupa espacio: vive dentro del gap.

```css
.articulo {
  column-count: 3;
  column-gap: 2rem;
  column-rule: 1px solid #ddd;
}
```

## column-gap

| Valor | Efecto |
|-------|--------|
| `normal` | Un espacio por defecto (~1em) |
| Longitud | `2rem`, `40px`: el espacio entre columnas |

`column-gap` es el mismo concepto que el [[07 Espaciado (gap) | `gap`]] de Flexbox/Grid, aplicado a columnas. De hecho, `gap` también funciona aquí.

## column-rule: la línea separadora

> [!info] Como border, pero entre columnas
> `column-rule` se define igual que un [[04 Shorthand border | borde]] (grosor, estilo, color), pero se dibuja **entre** las columnas, **dentro del gap**, sin ocupar espacio ni mover el texto:
> ```css
> column-rule: 1px solid #ccc;      /* shorthand */
> /* o por partes: */
> column-rule-width: 1px;
> column-rule-style: solid;
> column-rule-color: #ccc;
> ```

## La línea no ensancha el gap

> [!warning] La regla vive dentro del espacio, no lo agranda
> A diferencia de un borde (que ocupa espacio), `column-rule` se dibuja **en medio** del `column-gap` sin afectar al ancho de las columnas ni al gap. Si la línea es más gruesa que el gap, puede solaparse con el texto; conviene un `column-gap` suficiente para que la línea respire:
> ```css
> column-gap: 2rem;
> column-rule: 1px solid #ddd;   /* la línea va centrada en los 2rem */
> ```

## Ejemplo de uso

```css
/* Artículo con columnas separadas por un filete sutil */
.columnas {
  columns: 16rem;
  column-gap: 2.5rem;
  column-rule: 1px solid #e0e0e8;
}

/* Lista de enlaces en columnas con separación amplia */
.indice {
  column-width: 12rem;
  column-gap: 3rem;
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Da un `column-gap` cómodo (≥ 2rem) para que las columnas no se peguen.
> - Usa `column-rule` para un filete sutil que separe las columnas, estilo editorial.
> - Asegura que el `gap` sea mayor que el grosor de la regla para que respire.
> - `gap` (la propiedad general) también funciona como `column-gap` aquí.

## Errores comunes

> [!warning] Trampas
> - **Gap demasiado pequeño**: las columnas se leen como una sola masa de texto.
> - **Regla más gruesa que el gap**: se solapa con el texto.
> - **Esperar que la regla ocupe espacio**: vive dentro del gap.

## Notas relacionadas

- [[01 column-count y column-width]] — definir las columnas.
- [[07 Espaciado (gap)]] — el `gap`, mismo concepto.
- [[04 Shorthand border]] — la sintaxis que `column-rule` reutiliza.
