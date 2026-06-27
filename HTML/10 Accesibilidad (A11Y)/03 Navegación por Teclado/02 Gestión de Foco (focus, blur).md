---
title: Gestión de Foco — Mover el foco con intención
aliases:
  - focus management
  - focus blur
tags:
  - html
  - api/concepto
  - a11y
draft: false
order: 2
---

# Gestión de Foco (focus, blur)

> [!definicion]
> La **gestión del foco** consiste en mover el punto de foco del teclado de forma deliberada cuando la interfaz cambia: tras abrir un modal, tras enviar un formulario con errores, al cargar una nueva vista en una SPA. Un foco bien gestionado guía al usuario de teclado; uno descuidado lo deja perdido. El detalle de la API (`focus()`, eventos) pertenece al curso de JavaScript.

```js
// Al abrir un modal, mover el foco dentro
modal.querySelector("button").focus();

// Al cerrarlo, devolver el foco al disparador
botonQueAbrio.focus();
```

## focus y blur

| Concepto | Significa |
|----------|-----------|
| **focus** | El elemento recibe el foco (con `.focus()` o por `Tab`) |
| **blur** | El elemento pierde el foco |
| `:focus` (CSS) | Estilar el elemento enfocado |
| `:focus-visible` (CSS) | Estilar solo cuando el foco viene del teclado |

## Cuándo mover el foco

> [!info] Situaciones que exigen gestionar el foco
> | Situación | Mover el foco a… |
> |-----------|------------------|
> | Abrir un modal/diálogo | El primer elemento del modal (o el modal) |
> | Cerrar el modal | El elemento que lo abrió |
> | Enviar un formulario con errores | El primer campo con error (o el resumen) |
> | Cargar nueva vista en SPA | El encabezado de la nueva vista |
> | Eliminar un elemento de una lista | El siguiente (o anterior) elemento |
>
> En todos estos casos, si el foco no se mueve, el usuario de teclado se queda donde estaba (a veces en un elemento que ya no existe), desorientado.

## Devolver el foco

Un principio clave: cuando una interacción temporal termina (cerrar un modal, un menú), el foco debe **volver** al elemento que la inició. Así el usuario continúa desde donde estaba, sin saltar al inicio de la página.

## El foco perdido en SPAs

> [!warning] Las SPAs rompen el foco al navegar
> En una aplicación de una sola página, "navegar" no recarga: solo cambia el contenido con JavaScript. Para un usuario de teclado/lector, esto es un problema: el foco se queda en el enlace pulsado y **no se entera** de que la página cambió. La solución es **mover el foco** (y a menudo anunciar el cambio con una [[06 Regiones Vivas (aria-live, aria-atomic, aria-relevant) | región viva]]) al encabezado de la nueva vista tras navegar.

## Buenas prácticas

> [!tip] Recomendaciones
> - Mueve el foco al abrir interfaces temporales (modales, menús) y devuélvelo al cerrarlas.
> - Tras errores de formulario, lleva el foco al primer error.
> - En SPAs, gestiona el foco en cada cambio de ruta.
> - Usa `:focus-visible` para un anillo de foco que solo aparece con teclado.

## Errores comunes

> [!warning] Trampas
> - **No devolver el foco** al cerrar un modal: salta al inicio de la página.
> - **Foco en un elemento eliminado**: se pierde (vuelve al `<body>`).
> - **SPAs sin gestión de foco**: el usuario de teclado no percibe los cambios de vista.

## Notas relacionadas

- [[01 tabindex]] — `tabindex="-1"` para enfocar zonas no interactivas.
- [[04 Trampas de Foco (focus trapping)]] — mantener el foco dentro de un modal.
- [[06 Regiones Vivas (aria-live, aria-atomic, aria-relevant)]] — anunciar los cambios de vista.
