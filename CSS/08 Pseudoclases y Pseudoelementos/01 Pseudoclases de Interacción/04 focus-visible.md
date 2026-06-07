---
title: focus-visible — Foco solo por teclado
aliases:
  - ":focus-visible"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":focus-visible"
grupo: pseudoclase
draft: false
---

# :focus-visible

> [!definicion]
> `:focus-visible` selecciona un elemento enfocado **solo cuando el navegador considera que el indicador debe verse**: típicamente al enfocar con **teclado**, no con un clic de ratón. Resuelve el viejo dilema de `:focus` (el anillo persistente tras clic) mostrando el foco solo cuando es útil.

```css
button:focus-visible { outline: 2px solid #cba6f7; outline-offset: 2px; }
```

## El problema que resuelve

> [!info] El anillo solo cuando hace falta
> Con [[03 focus | `:focus`]], el anillo aparece **siempre**, incluso tras un clic de ratón (donde molesta a muchos). Pero quitarlo del todo rompe la accesibilidad por teclado. `:focus-visible` es la solución: el navegador aplica un **heurístico** y muestra el foco solo cuando el usuario probablemente lo necesita (navegación por teclado), no tras un clic deliberado de ratón:
> - Enfocar con **Tab** → `:focus-visible` se activa (anillo visible).
> - Hacer **clic** en un botón → `:focus` se activa, pero `:focus-visible` **no** (sin anillo).

## El patrón recomendado

> [!tip] :focus-visible para el anillo, :focus para campos
> La práctica moderna de foco accesible:
> ```css
> /* El anillo de foco solo cuando el navegador lo considera necesario (teclado) */
> :focus-visible {
>   outline: 2px solid #cba6f7;
>   outline-offset: 2px;
> }
> /* Opcional: quitar el outline por defecto en :focus para clics de ratón */
> :focus:not(:focus-visible) {
>   outline: none;
> }
> ```
> Esto da lo mejor de ambos mundos: usuarios de teclado ven el anillo, usuarios de ratón no lo ven tras un clic, y la accesibilidad se mantiene.

## Los campos de texto siempre lo muestran

> [!info] Los inputs muestran foco con cualquier entrada
> Un matiz del heurístico: los campos de **texto** (`<input>`, `<textarea>`) activan `:focus-visible` **también** al hacer clic, porque el usuario necesita ver dónde va a escribir. Es coherente: el anillo aparece cuando aporta información, y se omite solo en botones/enlaces clicados con ratón.

## Soporte

`:focus-visible` tiene **buen soporte** en todos los navegadores modernos. Para navegadores antiguos, el respaldo es un `:focus` normal (que muestra el anillo siempre, peor pero accesible). Es seguro usarlo hoy.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:focus-visible` para el **anillo de foco** (en vez de `:focus`).
> - Combínalo con `:focus:not(:focus-visible) { outline: none }` para quitar el anillo en clics de ratón.
> - Asegura un anillo de **alto contraste** y `outline-offset` para que respire.
> - **Nunca** quites el foco sin este reemplazo.

## Errores comunes

> [!warning] Trampas
> - **Usar `:focus`** para el anillo: persiste tras los clics de ratón.
> - **Quitar `:focus` sin poner `:focus-visible`**: inaccesible por teclado.
> - **Anillo de bajo contraste**: invisible para quien más lo necesita.

## Notas relacionadas

- [[03 focus]] — el foco con cualquier método de entrada.
- [[01 outline]] — el contorno del anillo.
- [[05 focus-within]] — un ancestro con descendiente enfocado.
