---
title: hidden — Ocultar un elemento
aliases:
  - hidden attribute
tags:
  - html
  - api/atributo
  - atributos
elemento: global
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
order: 5
---

# Ocultar (hidden)

> [!definicion]
> El atributo booleano `hidden` oculta un elemento **por completo**: no se muestra ni se anuncia a los lectores de pantalla, como si no estuviera. Equivale a `display: none`. Es la forma semántica de marcar contenido como "no relevante ahora".

```html
<div hidden>Este contenido está oculto para todos.</div>
```

## Oculta para todos

> [!info] hidden = invisible y no accesible
> `hidden` retira el elemento de la presentación **y** del árbol de accesibilidad: ni se ve ni lo leen los lectores de pantalla. Es lo correcto cuando el contenido **no debe percibirse en absoluto** en ese momento (un panel cerrado, un paso futuro de un formulario). Se diferencia de:
> | Técnica | ¿Visible? | ¿Lectores? |
> |---------|-----------|------------|
> | `hidden` / `display:none` | No | No |
> | `aria-hidden="true"` | Sí | No |
> | clase `sr-only` | No | **Sí** |

## hidden="until-found"

Un valor moderno, `hidden="until-found"`, oculta el contenido pero permite que el navegador lo **encuentre** con "buscar en la página" (Ctrl+F) y lo revele automáticamente. Útil para acordeones cuyo contenido debe ser buscable aunque esté plegado:

```html
<div hidden="until-found">Contenido plegado pero buscable.</div>
```

## CSS puede sobrescribirlo

> [!warning] Una regla CSS puede anular hidden
> `hidden` equivale a `display: none`, pero es solo el valor por defecto: **cualquier** regla CSS con `display` distinto lo anula. Un `.clase { display: block }` que afecte a un elemento con `hidden` lo **mostraría**. Si dependes de `hidden`, evita reglas que fijen `display` sobre esos elementos, o refuérzalo:
> ```css
> [hidden] { display: none !important; }
> ```

## hidden vs. controlar con JS

`hidden` se alterna fácilmente desde JavaScript (`elemento.hidden = true/false`), lo que lo hace cómodo para mostrar/ocultar paneles. Para un menú desplegable, se combina con [[05 Estados ARIA (expanded, hidden, checked, selected) | `aria-expanded`]] en el disparador.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `hidden` para contenido que no debe percibirse ahora (ni verse ni leerse).
> - Para ocultar **solo a la vista** manteniendo el acceso a lectores, usa la clase `sr-only`.
> - Alterna `hidden` con JS para paneles; sincroniza `aria-expanded` en el control.
> - Considera `hidden="until-found"` para contenido plegado que deba ser buscable.

## Errores comunes

> [!warning] Trampas
> - **Esperar que `hidden` lo lean los lectores**: no lo hacen (usa `sr-only` para eso).
> - **Una regla CSS de `display`** que anula `hidden` sin querer.
> - **Confundir `hidden` con `aria-hidden`**: el segundo oculta solo a la asistencia, no a la vista.

## Notas relacionadas

- [[05 Estados ARIA (expanded, hidden, checked, selected)]] — `aria-hidden`, que oculta solo a la asistencia.
- [[03 Contenido Oculto Visualmente (sr-only)]] — ocultar a la vista pero no a lectores.
- [[13 Inactivo (inert)]] — desactivar interacción sin ocultar.
