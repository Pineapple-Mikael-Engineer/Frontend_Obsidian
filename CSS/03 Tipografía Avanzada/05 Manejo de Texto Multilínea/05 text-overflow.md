---
title: text-overflow — Recortar con puntos suspensivos
aliases:
  - text-overflow
  - ellipsis
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: text-overflow
grupo: tipografia
valor_inicial: clip
hereda: false
animable: false
draft: false
---

# text-overflow

> [!definicion]
> `text-overflow` controla cómo se indica el texto **recortado** en **una sola línea**: cortarlo en seco (`clip`) o añadir puntos suspensivos (`ellipsis`). No funciona solo: necesita que el texto no ajuste y que el desbordamiento esté oculto.

```css
.titulo {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

## La receta de tres propiedades

> [!warning] text-overflow necesita nowrap + overflow hidden
> `text-overflow` por sí solo **no hace nada**: requiere las otras dos propiedades que crean las condiciones:
> ```css
> .recortar {
>   white-space: nowrap;       /* el texto no salta de línea */
>   overflow: hidden;          /* lo que sobra se oculta */
>   text-overflow: ellipsis;   /* y se marca con … */
> }
> ```
> - Sin `white-space: nowrap`, el texto ajustaría a varias líneas y no habría desbordamiento que recortar.
> - Sin `overflow: hidden`, el texto se saldría en vez de recortarse.
>
> Las tres juntas dan el "recorte con puntitos" de una línea.

## Valores

| Valor | Efecto |
|-------|--------|
| `clip` | Corta el texto en seco, sin indicador (por defecto) |
| `ellipsis` | Añade `…` donde se recorta |
| `"<cadena>"` | Un texto personalizado como indicador (soporte limitado) |

## Una línea vs. varias

> [!info] text-overflow es solo para una línea
> `text-overflow: ellipsis` recorta texto de **una sola línea**. Para recortar a **varias** líneas con puntos suspensivos, no sirve: hay que usar [[04 line-clamp | `-webkit-line-clamp`]], que es la herramienta multilínea. Es la confusión más común: intentar `text-overflow: ellipsis` en un párrafo de varias líneas (no funciona sin `nowrap`, que lo fuerza a una).

## Casos de uso

| Uso | Patrón |
|-----|--------|
| Título largo en una tarjeta estrecha | nowrap + ellipsis |
| Nombre de archivo en una lista | nowrap + ellipsis |
| Celda de tabla con texto que no debe envolver | nowrap + ellipsis |
| Ruta de breadcrumb que no cabe | nowrap + ellipsis |

## Accesibilidad del texto recortado

> [!warning] El texto recortado sigue ahí, pero el usuario no lo ve
> El texto cortado por `ellipsis` permanece en el DOM (accesible a lectores de pantalla y seleccionable), pero el usuario que **ve** la pantalla no puede leerlo. Para no ocultar información importante, conviene ofrecer el texto completo en `title` (tooltip), en hover, o de otra forma:
> ```html
> <span class="recortar" title="Texto completo aquí">Texto completo aquí</span>
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Combina siempre `white-space: nowrap` + `overflow: hidden` + `text-overflow: ellipsis`.
> - Para varias líneas, usa `-webkit-line-clamp`, no `text-overflow`.
> - Ofrece el texto completo (en `title` o al interactuar) si recortas información útil.
> - Útil para títulos, nombres y celdas donde el espacio es fijo.

## Errores comunes

> [!warning] Trampas
> - **Solo `text-overflow: ellipsis`**: no hace nada sin `nowrap` y `overflow: hidden`.
> - **Esperarlo en varias líneas**: usa `line-clamp`.
> - **Ocultar información** sin dar acceso al texto completo.

## Notas relacionadas

- [[04 line-clamp]] — recorte con `…` de **varias** líneas.
- [[13 white-space]] — `nowrap`, requisito de `text-overflow`.
- [[03 Información Emergente (title)]] — mostrar el texto completo en tooltip.
