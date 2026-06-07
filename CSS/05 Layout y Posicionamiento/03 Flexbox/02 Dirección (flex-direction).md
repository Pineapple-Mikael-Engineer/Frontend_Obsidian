---
title: flex-direction — Dirección del eje principal
aliases:
  - flex-direction
tags:
  - css
  - api/propiedad
  - layout
propiedad: flex-direction
grupo: flexbox
valor_inicial: row
hereda: false
animable: false
draft: false
---

# flex-direction

> [!definicion]
> `flex-direction` define la **dirección del eje principal** de un contenedor flex: si los items se colocan en **fila** (horizontal) o en **columna** (vertical), y en qué sentido. Cambiar la dirección **intercambia** el papel de `justify-content` y `align-items`.

```css
.fila    { display: flex; flex-direction: row; }      /* por defecto */
.columna { display: flex; flex-direction: column; }   /* apilados */
```

## Valores

| Valor | Eje principal | Items |
|-------|---------------|-------|
| `row` | Horizontal → | En fila, de izq. a der. (por defecto) |
| `row-reverse` | Horizontal ← | En fila, invertidos |
| `column` | Vertical ↓ | Apilados, de arriba abajo |
| `column-reverse` | Vertical ↑ | Apilados, invertidos |

## La clave: los ejes se intercambian

> [!warning] Con column, justify y align cambian de eje
> El concepto más importante: `justify-content` siempre actúa en el eje **principal** y `align-items` en el **transversal**. Como `flex-direction` define cuál es el principal, **cambiarla intercambia los ejes**:
> | | `row` (por defecto) | `column` |
> |--|---------------------|----------|
> | `justify-content` | **Horizontal** | **Vertical** |
> | `align-items` | **Vertical** | **Horizontal** |
>
> Por eso, para "centrar verticalmente" en una columna, usas `justify-content: center` (no `align-items`), lo contrario que en una fila. Este intercambio confunde mucho al principio.

## column: apilar elementos

`flex-direction: column` apila los items verticalmente, útil para layouts de columna donde además quieres las ventajas de flex (alineación, `gap`, crecimiento):

```css
/* Tarjeta con contenido apilado y un pie al fondo */
.tarjeta {
  display: flex;
  flex-direction: column;
  min-height: 20rem;
}
.tarjeta .pie { margin-top: auto; }   /* empuja el pie al fondo */
```

## El truco de margin-top: auto

> [!tip] Empujar un item al extremo
> En un contenedor `column`, `margin-top: auto` en un item lo empuja al **fondo** (absorbe todo el espacio sobrante encima). En `row`, `margin-left: auto` lo empuja a la **derecha**. Es una receta limpia para separar grupos:
> ```css
> /* En una navbar: el último grupo va a la derecha */
> .navbar .perfil { margin-left: auto; }
> ```

## reverse y la accesibilidad

> [!warning] reverse cambia el orden visual, no el del DOM
> `row-reverse`/`column-reverse` invierten el orden **visual**, pero el orden del **DOM** (y por tanto el de tabulación con teclado y el de lectura) **no cambia**. Esto puede crear una desconexión entre lo que se ve y el orden de foco, una trampa de accesibilidad. Úsalos con cuidado; si el orden importa de verdad, cámbialo en el HTML.

## Buenas prácticas

> [!tip] Recomendaciones
> - Recuerda que `column` **intercambia** los ejes de `justify`/`align`.
> - Usa `column` con `margin-top: auto` para empujar un pie al fondo de una tarjeta.
> - Evita `reverse` si el orden de foco/lectura importa (rompe la coherencia con el DOM).
> - Para cambiar la dirección por breakpoint, ajusta `flex-direction` en una media query.

## Errores comunes

> [!warning] Trampas
> - **Confundir los ejes** tras poner `column` (justify pasa a vertical).
> - **`reverse`** que desincroniza el orden visual del de tabulación.
> - **Usar column** cuando un simple apilado de bloques bastaría (no siempre hace falta flex).

## Notas relacionadas

- [[04 Eje Principal (justify-content)]] — actúa en el eje que define `flex-direction`.
- [[05 Eje Transversal (align-items, align-self)]] — el eje perpendicular.
- [[03 Ajuste de Línea (flex-wrap)]] — combinar dirección y wrap.
