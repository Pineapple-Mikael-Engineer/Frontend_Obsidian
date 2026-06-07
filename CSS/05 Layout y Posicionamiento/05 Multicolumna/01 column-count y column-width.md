---
title: column-count y column-width — Definir las columnas
aliases:
  - column-count
  - column-width
  - columns
tags:
  - css
  - api/propiedad
  - layout
propiedad: column-count
grupo: layout
valor_inicial: auto
hereda: false
animable: false
draft: false
---

# column-count y column-width

> [!definicion]
> `column-count` fija un **número** concreto de columnas; `column-width` fija un **ancho ideal** y deja que el navegador calcule cuántas caben. El atajo `columns` combina ambos. Definir uno de los dos activa el layout multicolumna.

```css
.fijo     { column-count: 3; }       /* exactamente 3 columnas */
.fluido   { column-width: 15rem; }   /* tantas columnas de ~15rem como quepan */
```

## Las dos formas

| Propiedad | Comportamiento |
|-----------|----------------|
| `column-count: 3` | Siempre **3 columnas**, de ancho variable según el contenedor |
| `column-width: 15rem` | Columnas de **~15rem**; el número se ajusta al ancho disponible |

## column-width es responsivo

> [!tip] column-width se adapta solo
> `column-width` es como un `minmax` para columnas de texto: el navegador mete tantas columnas del ancho indicado como quepan, **reduciendo el número en pantallas estrechas**. En móvil acaba en una sola columna, sin media queries:
> ```css
> .articulo { column-width: 18rem; }
> /* escritorio: 2-3 columnas; móvil: 1 columna, automático */
> ```
> Por eso `column-width` suele ser preferible a `column-count` para diseño responsivo: se adapta solo.

## El shorthand columns

`columns` combina ambos. Cuando das los dos, `column-count` actúa como **máximo**:

```css
columns: 15rem 3;
/* columnas de ~15rem, pero nunca más de 3 */
```

```css
columns: 3;        /* = column-count: 3 */
columns: 15rem;    /* = column-width: 15rem */
```

## Combinar count y width

> [!info] Width + count = ancho ideal con tope de columnas
> Dar ambos crea un comportamiento útil: columnas del ancho indicado, pero **limitadas** a un número máximo. Evita que en pantallas enormes salgan demasiadas columnas estrechas:
> ```css
> .lista { columns: 12rem 4; }   /* columnas de ~12rem, máximo 4 */
> ```

## Ejemplos de uso

```css
/* Lista larga de enlaces en columnas (cabe sin scroll) */
.indice { column-width: 12rem; column-gap: 2rem; }

/* Glosario de términos en 2-3 columnas */
.glosario { columns: 16rem 3; }

/* Galería de imágenes estilo "masonry" simple */
.masonry { column-width: 15rem; column-gap: 1rem; }
.masonry img { width: 100%; margin-bottom: 1rem; }
```

## El truco del masonry simple

> [!tip] Un masonry básico con columnas
> Multicolumna permite un layout tipo "masonry" (mosaico de alturas distintas) muy simple para imágenes: las imágenes fluyen llenando las columnas. No tiene el control de Grid, pero es trivial de implementar. **Cuidado**: el orden de las imágenes es por columnas (de arriba abajo en cada una), no por filas.

## Buenas prácticas

> [!tip] Recomendaciones
> - Prefiere `column-width` (responsivo automático) a `column-count` (número fijo).
> - Combina `columns: <ancho> <max>` para un ancho ideal con tope de columnas.
> - Úsalo para texto y listas, no para componentes (eso es Grid).
> - Recuerda el problema de lectura: mejor para contenido corto o listas.

## Errores comunes

> [!warning] Trampas
> - **`column-count` fijo** que da columnas demasiado estrechas en móvil.
> - **Usarlo para layout de componentes**: el contenido fluye, no se posiciona.
> - **Esperar orden por filas** en un masonry: el flujo es por columnas.

## Notas relacionadas

- [[02 column-gap y column-rule]] — el espacio y la línea entre columnas.
- [[04 break-inside]] — evitar que elementos se partan entre columnas.
- [[12 Patrones (auto-fit, auto-fill)]] — el `auto-fit` de Grid, conceptualmente parecido.
