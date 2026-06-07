---
title: backdrop — El fondo de modales y diálogos
aliases:
  - "::backdrop"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: "::backdrop"
grupo: pseudoelemento
draft: false
---

# ::backdrop

> [!definicion]
> `::backdrop` estiliza el **fondo** que se coloca detrás de un elemento mostrado en la **capa superior** (top layer): un `<dialog>` abierto con `showModal()` o un elemento en pantalla completa (Fullscreen API). Es la "cortina" semitransparente que cubre el resto de la página tras un modal.

```css
dialog::backdrop {
  background: rgb(0 0 0 / 0.5);
  backdrop-filter: blur(4px);
}
```

## La cortina del modal nativo

> [!tip] Oscurecer el fondo tras un dialog modal
> Cuando se abre un `<dialog>` con `showModal()`, el navegador genera automáticamente un `::backdrop` detrás de él. Estilarlo da la "cortina" que enfoca la atención en el modal:
> ```css
> dialog::backdrop {
>   background: rgb(0 0 0 / 0.5);   /* oscurece el fondo */
>   backdrop-filter: blur(4px);     /* y lo desenfoca */
> }
> ```
> Antes, esa cortina se hacía con un `<div>` overlay manual y JavaScript; con `<dialog>` nativo y `::backdrop`, el navegador la gestiona (incluido el cierre con Esc y el foco atrapado).

## Cuándo existe el backdrop

> [!info] Solo en la "top layer"
> `::backdrop` solo existe para elementos que el navegador coloca en la **capa superior** (top layer), por encima de todo lo demás sin depender de `z-index`:
> - Un `<dialog>` abierto con `showModal()` (no con `show()` ni el atributo `open` directo).
> - Un elemento en **pantalla completa** (`requestFullscreen()`).
> - Popovers (`popover` API) en algunos casos.
>
> Un `<dialog>` no modal (abierto con `show()`) **no** tiene backdrop.

## Animar la aparición

> [!tip] Animar el backdrop con el modal
> El `::backdrop` se puede animar para que aparezca/desaparezca con el modal, usando transiciones y `@starting-style` (para la entrada desde la capa superior):
> ```css
> dialog::backdrop {
>   background: rgb(0 0 0 / 0);
>   transition: background 0.3s;
> }
> dialog[open]::backdrop {
>   background: rgb(0 0 0 / 0.5);
> }
> ```
> Combinado con `transition-behavior: allow-discrete`, el backdrop hace un fundido al abrir/cerrar.

## Las ventajas del dialog nativo

> [!info] Por qué usar <dialog> + ::backdrop
> Frente al overlay manual con `<div>` y JS, el `<dialog>` nativo con `::backdrop` ofrece gratis: el atrapamiento del **foco** dentro del modal, el cierre con **Esc**, la gestión de la capa superior (sin guerras de `z-index`), y la accesibilidad básica. Es la forma moderna y recomendada de hacer modales.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `::backdrop` con `<dialog>` + `showModal()` para la cortina del modal.
> - Combínalo con `backdrop-filter: blur()` para un efecto de cristal.
> - Anímalo con transiciones y `@starting-style` para fundidos suaves.
> - Prefiere `<dialog>` nativo al overlay manual: foco, Esc y capa superior gratis.

## Errores comunes

> [!warning] Trampas
> - **Esperar `::backdrop`** en un `<dialog>` abierto con `show()` (no modal): no existe.
> - **Hacer un overlay manual** cuando `<dialog>` + `::backdrop` lo da nativo.
> - **Olvidar el contraste/oscurecimiento** suficiente para enfocar el modal.

## Notas relacionadas

- [[04 fixed]] — el posicionamiento de los overlays manuales (alternativa antigua).
- [[02 backdrop-filter]] — el desenfoque del fondo.
- [[03 Roles de Widget]] — la accesibilidad de los diálogos.
