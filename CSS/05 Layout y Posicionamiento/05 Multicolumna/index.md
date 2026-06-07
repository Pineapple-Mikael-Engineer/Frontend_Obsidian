---
title: Multicolumna — Texto en columnas tipo periódico
aliases:
  - multicol
  - css columns
tags:
  - css
  - api/concepto
  - layout
draft: false
---

# Multicolumna

> [!definicion]
> El **layout multicolumna** reparte el **contenido de un elemento** (sobre todo texto) en varias **columnas** que fluyen como las de un periódico: el texto llena la primera columna, salta a la segunda, y así. Es distinto de Grid/Flexbox: aquí no colocas elementos, sino que **divides un flujo de texto** en columnas.

```css
.articulo {
  column-count: 3;
  column-gap: 2rem;
}
```

## Mapa de la subsección

- [[01 column-count y column-width]] — cuántas columnas o de qué ancho.
- [[02 column-gap y column-rule]] — el espacio y la línea entre columnas.
- [[03 column-span]] — que un elemento abarque todas las columnas.
- [[04 break-inside]] — evitar que un elemento se parta entre columnas.

## Cuándo usar multicolumna (y cuándo no)

> [!info] Para flujos de texto, no para layout de componentes
> Multicolumna es ideal para **texto largo** que debe leerse en columnas (un artículo, una lista larga de enlaces, términos). **No** es para maquetar componentes o rejillas de tarjetas (eso es Grid/Flexbox): en multicolumna el contenido fluye de una columna a otra de forma **continua**, lo que no encaja con elementos que deben tener posiciones definidas.

| Usar multicolumna | Usar Grid/Flexbox |
|-------------------|-------------------|
| Un texto largo en columnas | Una rejilla de tarjetas |
| Una lista de muchos enlaces | Componentes con posición fija |
| Términos de un glosario | Un layout de página |

## El inconveniente de leer en columnas en web

> [!warning] Las columnas en pantalla tienen un problema de lectura
> En papel, las columnas funcionan porque el ojo recorre una columna corta. En **pantalla**, si las columnas son altas, el usuario tiene que hacer **scroll hacia abajo** para terminar la primera columna y luego **volver arriba** para la segunda, una experiencia incómoda. Por eso el multicolumna en web se usa con cuidado: mejor para contenido **corto** (que cabe sin scroll) o listas, no para artículos largos.

## Notas relacionadas

- [[01 column-count y column-width]] — el punto de partida.
- [[04 CSS Grid/index]] — para layout de componentes (no texto en columnas).
- [[04 break-inside]] — evitar cortes feos entre columnas.
