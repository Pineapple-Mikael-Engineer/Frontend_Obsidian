---
title: focus-within — Un ancestro con descendiente enfocado
aliases:
  - ":focus-within"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":focus-within"
grupo: pseudoclase
draft: false
---

# :focus-within

> [!definicion]
> `:focus-within` selecciona un elemento cuando **él mismo o cualquiera de sus descendientes** tiene el foco. Permite estilizar un contenedor entero según si algo dentro está enfocado: resaltar un formulario cuando se edita un campo, mantener abierto un menú mientras se navega por él.

```css
.formulario:focus-within { border-color: #cba6f7; }
```

## Estilar el padre según el hijo enfocado

> [!tip] El contenedor reacciona al foco interno
> La idea clave: el contenedor responde cuando algo **dentro** de él tiene el foco, sin JavaScript:
> ```css
> /* Un campo de búsqueda que se resalta al editar su input */
> .busqueda:focus-within {
>   box-shadow: 0 0 0 3px rgb(203 166 247 / 0.3);
>   border-color: #cba6f7;
> }
> ```
> Cuando el usuario hace foco en el `<input>` interno, todo el `.busqueda` se resalta. Es un patrón muy limpio para feedback de "estoy editando esta zona".

## Menús desplegables accesibles

> [!tip] Mantener un menú abierto mientras se navega
> Un uso excelente para accesibilidad: un menú que se abre en `:hover` debe **seguir abierto** mientras el usuario navega por sus enlaces con el teclado. `:focus-within` lo logra:
> ```css
> .menu:hover .submenu,
> .menu:focus-within .submenu {
>   display: block;   /* abierto al pasar el ratón O al tener foco dentro */
> }
> ```
> Sin `:focus-within`, el submenú se cerraría al tabular hacia sus enlaces (el `:hover` del padre se pierde). Es lo que hace navegables por teclado los menús que antes solo funcionaban con ratón.

## Combinar con :hover

`:focus-within` se empareja casi siempre con `:hover` para cubrir ambas formas de interacción: ratón (hover) y teclado (focus-within). Es la pareja que hace accesibles los componentes interactivos:

```css
.dropdown:hover .panel,
.dropdown:focus-within .panel { opacity: 1; visibility: visible; }
```

## Formularios reactivos

```css
/* Resaltar el grupo de campos que se está editando */
.campo:focus-within label { color: #cba6f7; font-weight: 600; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:focus-within` para resaltar contenedores cuyo contenido se edita.
> - Empareja `:hover` + `:focus-within` para menús/desplegables accesibles.
> - Es la clave para que los componentes de hover funcionen también con teclado.
> - Combínalo con labels para indicar el grupo de campos activo.

## Errores comunes

> [!warning] Trampas
> - **Menú solo con `:hover`** que se cierra al tabular: añade `:focus-within`.
> - **Olvidar la pareja teclado/ratón**: usa ambos `:hover` y `:focus-within`.

## Notas relacionadas

- [[03 focus]] · [[04 focus-visible]] — el foco del descendiente.
- [[01 hover]] — la pareja para ratón.
- [[03 has()]] — la versión más general de "padre según hijo".
