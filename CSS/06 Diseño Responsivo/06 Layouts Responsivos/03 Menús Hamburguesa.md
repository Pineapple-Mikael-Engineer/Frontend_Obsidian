---
title: Menús Hamburguesa — El menú que se colapsa en móvil
aliases:
  - hamburger menu
tags:
  - css
  - api/concepto
  - responsive
draft: false
---

# Menús Hamburguesa

> [!definicion]
> El **menú hamburguesa** es el patrón donde, en pantallas pequeñas, la navegación se **colapsa** detrás de un botón (el icono de tres líneas ☰) y se despliega al tocarlo. Es un **cambio estructural** que sí requiere media queries (y normalmente JavaScript), porque no se puede interpolar de forma fluida.

```css
.nav-toggle { display: none; }           /* el botón se oculta en escritorio */
@media (max-width: 767px) {
  .nav-toggle { display: block; }        /* aparece en móvil */
  .nav-menu { display: none; }           /* el menú se oculta */
  .nav-menu.abierto { display: flex; }   /* se muestra al tocar (JS añade la clase) */
}
```

## Por qué necesita media queries (y JS)

> [!info] Un cambio estructural, no gradual
> A diferencia de una rejilla que se reorganiza sola, el menú hamburguesa es un cambio **estructural**: en escritorio es una barra horizontal visible; en móvil, un botón que despliega un panel. No hay "estado intermedio" que interpolar. Por eso:
> - La **media query** decide cuándo mostrar el botón y ocultar el menú.
> - El **JavaScript** alterna la clase que despliega/oculta el panel al tocar el botón (y gestiona el foco, `aria-expanded`, cerrar con Esc).

## La estructura accesible

> [!tip] El botón debe ser accesible
> Un menú hamburguesa accesible necesita más que el icono:
> ```html
> <button class="nav-toggle" aria-expanded="false" aria-controls="nav-menu" aria-label="Menú">
>   <svg aria-hidden="true">☰</svg>
> </button>
> <nav id="nav-menu" class="nav-menu">…</nav>
> ```
> - **`aria-label`** o texto oculto (el icono solo no basta).
> - **`aria-expanded`** que el JS actualiza (`true`/`false`) al abrir/cerrar.
> - **`aria-controls`** apuntando al menú.
> - El JS gestiona el **foco** (al panel al abrir, devuelto al cerrar) y el cierre con **Esc**.
>
> Detalle de la accesibilidad en [[03 Roles de Widget | widgets ARIA]].

## ¿Hace falta JS? Variantes solo-CSS

> [!info] El truco del checkbox (con reservas)
> Existe una versión **sin JavaScript** usando un checkbox oculto y el selector [[03 Hermano Adyacente (+) | `:checked +`]]:
> ```css
> #nav-toggle:checked ~ .nav-menu { display: flex; }
> ```
> Funciona, pero es un **hack** con limitaciones de accesibilidad (el estado no se comunica bien a la asistencia, gestión de foco pobre). Para un menú serio, **JavaScript** con los atributos ARIA correctos es preferible. El truco CSS sirve para prototipos o casos muy simples.

## No ocultes el menú en escritorio por error

> [!warning] La media query correcta
> Un error común: que la lógica del hamburguesa (ocultar el menú) se aplique también en escritorio. Asegúrate de que el menú esté **visible por defecto** (escritorio) y solo se colapse dentro de la media query de móvil:
> ```css
> .nav-menu { display: flex; }            /* visible por defecto (escritorio) */
> @media (max-width: 767px) {
>   .nav-menu { display: none; }          /* colapsado solo en móvil */
> }
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Menú visible por defecto (escritorio); colapsado dentro de la media query de móvil.
> - Botón con `aria-label`, `aria-expanded` (actualizado por JS) y `aria-controls`.
> - Gestiona el foco y el cierre con Esc en JavaScript.
> - Prefiere JS con ARIA al truco del checkbox para un menú accesible de verdad.

## Errores comunes

> [!warning] Trampas
> - **Botón de solo icono sin nombre** accesible.
> - **`aria-expanded` que no cambia**: el estado miente a la asistencia.
> - **Menú oculto también en escritorio** por una media query mal hecha.
> - **Sin gestión de foco**: el usuario de teclado se pierde al abrir el menú.

## Notas relacionadas

- [[03 Roles de Widget]] — la accesibilidad del menú desplegable.
- [[02 Gestión de Foco (focus, blur)]] — mover el foco al abrir/cerrar.
- [[03 Navegación (nav)]] — el elemento `<nav>` del menú.
