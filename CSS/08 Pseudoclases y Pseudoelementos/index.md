---
title: Pseudoclases y Pseudoelementos — Seleccionar estados y partes
aliases:
  - pseudo-classes
  - pseudo-elements
tags:
  - css
  - api/concepto
  - selectores
draft: false
order: 8
---

# Pseudoclases y Pseudoelementos

> [!definicion]
> Las **pseudoclases** (`:hover`, `:nth-child`) seleccionan elementos según su **estado** o **posición**, algo que el HTML no marca explícitamente. Los **pseudoelementos** (`::before`, `::placeholder`) estilizan **partes** de un elemento que no son nodos del DOM. Juntos amplían enormemente lo que un selector puede alcanzar.

```css
a:hover { color: #cba6f7; }            /* pseudoclase: estado */
li:nth-child(odd) { background: #eee; } /* pseudoclase: posición */
.cita::before { content: "“"; }         /* pseudoelemento: parte generada */
```

## Pseudoclase vs. pseudoelemento

> [!info] Un dos puntos vs. dos dos puntos
> La diferencia y la convención de sintaxis:
> - **Pseudoclase** (`:`): selecciona un elemento **completo** en cierto estado o posición (`:hover`, `:first-child`, `:checked`). Un solo `:`.
> - **Pseudoelemento** (`::`): estiliza una **parte** del elemento o crea contenido que no existe en el DOM (`::before`, `::first-line`, `::placeholder`). Dos `::`.
>
> La convención moderna usa `::` para pseudoelementos y `:` para pseudoclases (aunque los pseudoelementos antiguos como `:before` aún funcionan con uno, por compatibilidad).

## Mapa de la sección

- [[01 Pseudoclases de Interacción/index]] — `:hover`, `:focus`, `:focus-visible`, `:target`…
- [[02 Pseudoclases de Estructura/index]] — `:nth-child`, `:first-child`, `:empty`, `:root`…
- [[03 Pseudoclases de Formulario/index]] — `:checked`, `:disabled`, `:valid`, `:user-invalid`…
- [[04 Pseudoclases Funcionales/index]] — `:is()`, `:where()`, `:has()`, `:not()`.
- [[05 Pseudoelementos/index]] — `::before`, `::after`, `::placeholder`, `::marker`…

## Estilar sin tocar el HTML ni el JS

> [!tip] Estados y partes, solo con CSS
> El gran valor de esta sección: estilizar **estados** (foco, marcado, inválido) y **partes** (primera letra, marcador de lista, placeholder) **sin** añadir clases con JavaScript ni envolturas en el HTML. Un `:hover` reacciona sin un `mouseover` en JS; un `::before` añade un icono sin un `<span>` extra; `:has()` permite estilar un padre según su hijo, algo antes imposible sin JS. Reducen la dependencia de JavaScript para la presentación.

## El salto de :has()

> [!info] El "selector de padre" que cambió las reglas
> Durante años, CSS solo podía seleccionar **hacia abajo** (un hijo según su padre). [[03 has() | `:has()`]] introdujo la selección **hacia arriba** (un padre según su hijo) y relaciones laterales, habilitando patrones que antes exigían JavaScript. Es la incorporación más transformadora de los selectores modernos.

## Notas relacionadas

- [[01 Pseudoclases de Interacción/index]] — los estados de interacción.
- [[05 Pseudoelementos/index]] — estilizar partes generadas.
- [[02 Especificidad de Pseudoclases y Pseudoelementos]] — cómo pesan en la cascada.
