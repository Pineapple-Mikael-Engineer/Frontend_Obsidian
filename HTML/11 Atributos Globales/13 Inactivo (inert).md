---
title: inert — Desactivar una rama del DOM
aliases:
  - inert
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

# Inactivo (inert)

> [!definicion]
> El atributo booleano `inert` vuelve un elemento y **todo su contenido** completamente **inactivo**: no recibe foco, no responde a clics, y los lectores de pantalla lo ignoran. A diferencia de [[05 Ocultar (hidden) | `hidden`]], el contenido **sigue visible**; solo deja de ser interactivo. Es la herramienta clave para el contenido **de fondo** de un modal.

```html
<main inert>
  <!-- visible pero inactivo mientras el modal está abierto -->
</main>
<dialog open>…</dialog>
```

## El caso de uso: el fondo de un modal

> [!info] El problema que resuelve
> Cuando se abre un modal, el contenido de detrás sigue ahí. Sin `inert`, el usuario de teclado puede **tabular** hacia ese fondo (saliéndose del modal), y un lector de pantalla puede leerlo, rompiendo la sensación de que el modal "captura" toda la atención. `inert` sobre el fondo lo desactiva por completo:
> - No se puede enfocar ni hacer clic en nada de detrás.
> - Los lectores de pantalla no lo leen.
> - Pero sigue **visible** (atenuado con CSS), a diferencia de `hidden`.
>
> Es la pieza que faltaba para hacer modales accesibles sin trucos de [[04 Trampas de Foco (focus trapping) | trampa de foco]] manual.

## inert vs. hidden vs. disabled

| | `inert` | `hidden` | `disabled` |
|--|---------|----------|------------|
| ¿Visible? | **Sí** | No | Sí |
| ¿Interactivo? | No | No | No |
| ¿Lo leen lectores? | No | No | Depende |
| Alcance | El elemento y **todo** su contenido | El elemento | Solo controles de formulario |

`inert` es el único que deja el contenido **visible pero totalmente inerte**, y se aplica a cualquier elemento (no solo controles, como `disabled`).

## dialog lo hace solo

El elemento [[01 Marco en Línea (iframe) | nativo]] `<dialog>` abierto con `showModal()` aplica el efecto inerte al resto de la página **automáticamente**. Por eso es la forma recomendada de hacer modales: no hace falta poner `inert` a mano.

## Estilar el contenido inerte

Conviene dar una señal visual de que el fondo está desactivado (atenuarlo), ya que `inert` no cambia la apariencia por sí solo:

```css
[inert] { opacity: 0.5; pointer-events: none; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `inert` para desactivar el contenido de fondo de un modal a medida.
> - Prefiere `<dialog>` + `showModal()`, que aplica la inercia solo.
> - Añade una señal visual (atenuar) al contenido inerte.
> - No olvides **quitar** `inert` al cerrar el modal.

## Errores comunes

> [!warning] Trampas
> - **Olvidar quitar `inert`** al cerrar: el fondo queda inutilizable.
> - **Confundir `inert` con `hidden`**: `inert` mantiene el contenido visible.
> - **Reinventar la trampa de foco** a mano cuando `inert` (o `<dialog>`) lo resuelve.

## Notas relacionadas

- [[05 Ocultar (hidden)]] — ocultar por completo, frente a inert (visible pero inactivo).
- [[04 Trampas de Foco (focus trapping)]] — `inert` es la solución moderna al fondo del modal.
- [[03 Roles de Widget]] — el patrón de diálogo modal.
