---
title: draggable — Hacer un elemento arrastrable
aliases:
  - draggable
  - drag and drop
tags:
  - html
  - api/atributo
  - atributos
elemento: global
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
---

# Arrastrable (draggable)

> [!definicion]
> `draggable` indica si un elemento se puede **arrastrar** con el ratón (drag and drop). Es la pieza HTML de la **API de Drag and Drop**: marca el elemento como arrastrable, pero todo el comportamiento (qué pasa al arrastrar y soltar) se programa con JavaScript.

```html
<div draggable="true" id="ficha">Arrástrame</div>
```

## Valores

| Valor | Efecto |
|-------|--------|
| `true` | El elemento se puede arrastrar |
| `false` | No se puede arrastrar |
| `auto` (por defecto) | Comportamiento del navegador (imágenes y enlaces son arrastrables solos) |

> [!info] Algunos elementos ya son draggable
> Por defecto, las imágenes ([[01 Imagen (img) | `<img>`]]) y los enlaces ([[01 Enlaces (a) | `<a>`]]) **ya son arrastrables**. `draggable="false"` los desactiva; `draggable="true"` activa el resto de elementos.

## Solo marca; el comportamiento es JS

> [!warning] draggable no hace nada por sí solo
> Poner `draggable="true"` solo permite **iniciar** el arrastre; no define qué ocurre. La funcionalidad completa exige manejar eventos en JavaScript:
> | Evento | Cuándo |
> |--------|--------|
> | `dragstart` | Al empezar a arrastrar (guardar el dato) |
> | `dragover` | Sobre una zona de destino (permitir soltar) |
> | `drop` | Al soltar (procesar) |
> | `dragend` | Al terminar |
>
> El desarrollo de la API pertenece al curso de JavaScript.

## Accesibilidad: el gran problema

> [!warning] El drag and drop nativo no es accesible por teclado
> La API de arrastrar y soltar funciona **solo con ratón/táctil**: no hay forma nativa de arrastrar con teclado, lo que excluye a quienes no usan ratón. Una interfaz de arrastre accesible debe ofrecer una **alternativa** (botones "subir/bajar", un campo para introducir la posición, atajos). Por eso, antes de un drag and drop a medida, conviene valorar si una lista con botones de reordenar no sería más accesible y simple.

## Buenas prácticas

> [!tip] Recomendaciones
> - `draggable="true"` solo marca; implementa los eventos en JS.
> - **Ofrece siempre una alternativa accesible** por teclado a cualquier acción de arrastre.
> - Para reordenar, valora botones o campos en lugar de (o además de) arrastrar.
> - Da feedback visual claro de las zonas donde se puede soltar.

## Errores comunes

> [!warning] Trampas
> - **Esperar que `draggable` funcione solo**: necesita JS.
> - **Drag and drop sin alternativa de teclado**: inaccesible.
> - **No prevenir el comportamiento por defecto** en `dragover` (sin ello, `drop` no se dispara).

## Notas relacionadas

- [[09 Editable (contenteditable)]] — otro atributo que habilita interacción avanzada.
- [[03 Navegación por Teclado/index]] — por qué hace falta alternativa de teclado.
