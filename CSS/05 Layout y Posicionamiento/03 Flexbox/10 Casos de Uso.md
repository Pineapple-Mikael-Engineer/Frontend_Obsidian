---
title: Casos de Uso de Flexbox — Recetas listas
aliases:
  - flexbox patterns
  - flexbox recipes
tags:
  - css
  - api/concepto
  - layout
draft: false
---

# Casos de Uso de Flexbox

> [!definicion]
> Flexbox resuelve un puñado de patrones de layout que aparecen una y otra vez. Esta nota reúne las **recetas** más útiles, listas para copiar y adaptar. Casi cualquier componente lineal de una interfaz es una variación de estas.

## Centrar cualquier cosa

```css
.centro {
  display: flex;
  justify-content: center;   /* horizontal */
  align-items: center;       /* vertical */
}
```
El centrado perfecto en ambos ejes, sin importar el tamaño del contenido. La solución a un problema que atormentó a los desarrolladores durante años.

## Barra de navegación (logo + menú)

```css
.navbar {
  display: flex;
  justify-content: space-between;   /* logo a la izquierda, resto a la derecha */
  align-items: center;
  gap: 1rem;
}
```
`space-between` empuja los extremos a los bordes. Si el menú debe ir a la derecha y hay varios grupos, `margin-left: auto` en el grupo que debe separarse.

## Icono junto a texto

```css
.con-icono {
  display: flex;
  align-items: center;   /* alinea el icono con el texto */
  gap: 0.5rem;
}
.con-icono svg { flex-shrink: 0; }   /* el icono no se aplasta */
```

## Tarjeta con pie al fondo

```css
.tarjeta {
  display: flex;
  flex-direction: column;
  height: 100%;
}
.tarjeta .pie { margin-top: auto; }   /* empuja el pie abajo del todo */
```
En un grid de tarjetas de igual altura, esto alinea todos los pies (botón, precio) en la base, sin importar cuánto contenido tenga cada una.

## Columnas de igual ancho

```css
.columnas {
  display: flex;
  gap: 1rem;
}
.columnas > * { flex: 1; }   /* todas iguales */
```

## Rejilla fluida (se reorganiza sola)

```css
.galeria {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}
.galeria > * {
  flex: 1 1 15rem;   /* ideal 15rem; crece, encoge y salta de línea */
}
```
Cada item busca medir ~15rem; cuando no caben, saltan a la siguiente fila. Responsivo sin media queries. (Para columnas perfectamente alineadas, [[02 Grid con auto-fit | Grid con `auto-fit`]].)

## Lista de etiquetas (chips)

```css
.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}
```

## Formulario en línea (campo + botón)

```css
.busqueda {
  display: flex;
  gap: 0.5rem;
}
.busqueda input { flex: 1; min-width: 0; }   /* el input ocupa el resto */
.busqueda button { flex-shrink: 0; }          /* el botón mantiene su tamaño */
```

## "Media object" (avatar + contenido)

```css
.media {
  display: flex;
  gap: 1rem;
}
.media .avatar { flex-shrink: 0; }   /* tamaño fijo */
.media .cuerpo { flex: 1; min-width: 0; }   /* el resto, sin desbordar */
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Memoriza el centrado (`justify-content: center; align-items: center`) y `space-between` para navbars.
> - `margin-top: auto` (column) o `margin-left: auto` (row) para empujar un item al extremo.
> - `flex-shrink: 0` en iconos/avatares; `flex: 1; min-width: 0` en el contenido flexible.
> - Para rejillas con columnas alineadas y control 2D, pasa a Grid.

## Notas relacionadas

- [[01 Contenedor Flex (display flex)]] — el punto de partida de todas las recetas.
- [[09 flex-grow, flex-shrink, flex-basis]] — el reparto de espacio de las rejillas.
- [[04 CSS Grid/index]] — para layouts bidimensionales.
