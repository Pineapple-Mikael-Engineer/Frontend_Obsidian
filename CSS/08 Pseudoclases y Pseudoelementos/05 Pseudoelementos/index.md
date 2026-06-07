---
title: Pseudoelementos — Estilar partes generadas o no marcadas
aliases:
  - pseudo-elements
tags:
  - css
  - api/concepto
  - selectores
draft: false
---

# Pseudoelementos

> [!definicion]
> Los **pseudoelementos** estilizan **partes** de un elemento que no son nodos del DOM: la primera letra (`::first-letter`), el marcador de una lista (`::marker`), el texto de relleno de un campo (`::placeholder`), o **contenido generado** que no existe en el HTML (`::before`, `::after`). Se escriben con **dos dos puntos** (`::`).

```css
.cita::before { content: "“"; }
input::placeholder { color: #999; }
li::marker { color: #cba6f7; }
```

## Mapa de la subsección

- [[01 before y after]] — contenido generado antes/después.
- [[02 Propiedad content]] — qué genera `::before`/`::after`.
- [[03 first-letter]] — la primera letra (capitulares).
- [[04 first-line]] — la primera línea.
- [[05 selection]] — el texto seleccionado por el usuario.
- [[06 placeholder]] — el texto de relleno de los campos.
- [[07 marker]] — el marcador de las listas.
- [[08 backdrop]] — el fondo de modales/diálogos a pantalla completa.
- [[09 file-selector-button]] — el botón de los inputs de archivo.

## La sintaxis: dos dos puntos

> [!info] :: para pseudoelementos, : para pseudoclases
> La convención moderna distingue:
> - **Pseudoclase** (`:`): un elemento en cierto **estado** (`:hover`, `:checked`).
> - **Pseudoelemento** (`::`): una **parte** del elemento (`::before`, `::first-line`).
>
> Los pseudoelementos antiguos (`::before`, `::after`, `::first-letter`, `::first-line`) aún funcionan con **un** solo `:` por compatibilidad histórica, pero la forma correcta es `::`. Los nuevos (`::placeholder`, `::marker`) **exigen** los dos.

## Solo uno por selector (clásicamente)

> [!warning] Un pseudoelemento por selector
> Tradicionalmente, solo se puede usar **un** pseudoelemento por selector (no se pueden encadenar `::before::after`). Cada uno se aplica por separado. Hay propuestas para anidarlos (`::before::marker`), pero el soporte es limitado.

## El más versátil: ::before y ::after

> [!tip] Contenido decorativo sin ensuciar el HTML
> [[01 before y after | `::before` y `::after`]] son los más usados: insertan contenido (iconos, comillas, etiquetas, formas decorativas) **sin** añadir elementos al HTML. Mantienen el marcado limpio y semántico, dejando lo puramente decorativo en el CSS. Requieren la propiedad [[02 Propiedad content | `content`]] para existir.

## Notas relacionadas

- [[01 before y after]] — los más usados.
- [[06 placeholder]] · [[07 marker]] — estilar partes de campos y listas.
- [[02 Especificidad de Pseudoclases y Pseudoelementos]] — su peso en la cascada.
