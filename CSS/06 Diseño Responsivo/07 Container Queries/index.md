---
title: Container Queries — Adaptar al contenedor, no al viewport
aliases:
  - container queries
tags:
  - css
  - api/concepto
  - responsive
draft: false
order: 6
---

# Container Queries

> [!definicion]
> Las **container queries** permiten que un componente se adapte al tamaño de **su contenedor**, no del viewport. Es el avance más importante del responsive reciente: por fin un componente "sabe" en qué espacio está (¿toda la pantalla o una columna estrecha?) y se ajusta en consecuencia, haciéndolo verdaderamente **reutilizable**.

```css
.tarjeta { container-type: inline-size; }

@container (min-width: 25rem) {
  .tarjeta .contenido { display: flex; }   /* fila si el contenedor es ancho */
}
```

## El problema que resuelven

> [!info] El componente no conocía su contexto
> Con [[02 Media Queries/index | media queries]], un componente reacciona al **viewport** (la ventana), pero **no sabe** cuánto espacio ocupa **él**. La misma tarjeta puede estar en una columna ancha (donde cabe en horizontal) o en una barra lateral estrecha (donde debe apilarse), pero el viewport es el mismo en ambos casos. Resultado: el componente no podía adaptarse a su **contexto real**, solo al tamaño de la pantalla. Las container queries resuelven exactamente esto: la tarjeta se adapta a **su contenedor**, donde sea que esté.

## Mapa de la subsección

- [[01 @container y container-type]] — declarar un contenedor y consultarlo.
- [[02 Unidades de Contenedor (cqw, cqh)]] — dimensionar respecto al contenedor.

## El cambio de paradigma

> [!tip] Componentes verdaderamente reutilizables
> Las container queries permiten escribir un componente **una vez** y que funcione bien **en cualquier contexto**: ancho completo, media columna, barra lateral. El mismo `.tarjeta` se ve en horizontal donde hay espacio y apilado donde no, sin saber nada del layout de la página. Esto es la base del diseño verdaderamente **modular** y es por lo que se considera el mayor avance del CSS responsivo en años.

## Container queries + media queries

> [!info] No sustituyen, complementan
> Las container queries no eliminan las media queries: las complementan.
> - **Media queries**: para decisiones a nivel de **página** (el layout global, el menú hamburguesa).
> - **Container queries**: para que cada **componente** se adapte a su espacio.
>
> Un layout de página con media queries que coloca componentes, y cada componente con container queries que se adapta a la celda donde cae. Esa combinación es el responsive moderno.

## Soporte

> [!info] Soporte ya generalizado
> Las container queries llegaron en 2023 y hoy tienen **buen soporte** en todos los navegadores principales. Para proyectos que deban soportar navegadores antiguos, conviene un respaldo (media queries), pero en la web actual son seguras de usar.

## Notas relacionadas

- [[01 @container y container-type]] — el punto de partida.
- [[02 Media Queries/index]] — el sistema basado en viewport, complementario.
- [[02 Unidades de Contenedor (cqw, cqh)]] — las unidades relativas al contenedor.
