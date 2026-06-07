---
title: focus — El elemento enfocado
aliases:
  - ":focus"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":focus"
grupo: pseudoclase
draft: false
---

# :focus

> [!definicion]
> `:focus` selecciona el elemento que tiene el **foco**: el que recibe la entrada de teclado en ese momento (un campo donde se escribe, un botón seleccionado con Tab). Es fundamental para la accesibilidad: marca dónde está el usuario que navega con teclado.

```css
input:focus { border-color: #cba6f7; }
```

## Qué recibe foco

Reciben foco de forma natural los elementos **interactivos**: enlaces (`<a href>`), botones, campos de formulario, `<select>`, `<textarea>`. Otros elementos pueden recibirlo con [[01 tabindex | `tabindex`]].

## Nunca elimines el foco sin reemplazo

> [!warning] outline: none rompe la navegación por teclado
> El error de accesibilidad más extendido: quitar el indicador de foco (`:focus { outline: none }`) porque "se ve feo", dejando a quien navega con teclado **sin saber dónde está**. Si el outline por defecto no encaja, **reemplázalo**, nunca lo elimines:
> ```css
> /* ❌ inaccesible */
> :focus { outline: none; }
> /* ✅ reemplazar por uno propio */
> :focus-visible { outline: 2px solid #cba6f7; outline-offset: 2px; }
> ```

## El problema de :focus con el ratón

> [!info] :focus se activa también al hacer clic
> `:focus` se aplica con **cualquier** forma de enfocar, incluido un **clic de ratón**. Esto molesta: tras hacer clic en un botón, queda con el "anillo de foco" persistente hasta que se hace clic en otro sitio, algo que muchos usuarios de ratón perciben como un error visual. La solución moderna es [[04 focus-visible | `:focus-visible`]], que muestra el anillo **solo** cuando el foco viene del teclado:
> ```css
> /* En vez de :focus, usar :focus-visible para el anillo */
> button:focus-visible { outline: 2px solid #cba6f7; }
> ```

## :focus para estilos de campo activo

`:focus` sigue siendo correcto para resaltar un **campo activo** (un input donde se está escribiendo), donde el feedback es deseable con cualquier método de entrada:

```css
input:focus { border-color: #cba6f7; box-shadow: 0 0 0 3px rgb(203 166 247 / 0.2); }
```

## focus-within y focus-visible

`:focus` tiene dos parientes muy útiles: [[05 focus-within | `:focus-within`]] (un ancestro cuando algún descendiente tiene el foco) y [[04 focus-visible | `:focus-visible`]] (foco solo por teclado). Para el **anillo de foco**, usa `:focus-visible`; `:focus` para resaltar campos.

## Buenas prácticas

> [!tip] Recomendaciones
> - **Nunca** elimines el foco sin reemplazo: usa `:focus-visible` para el anillo.
> - `:focus` para resaltar campos activos (deseable con cualquier entrada).
> - `:focus-visible` para el anillo de foco (solo teclado).
> - Asegura contraste suficiente del indicador de foco.

## Errores comunes

> [!warning] Trampas
> - **`outline: none`** sin alternativa: inaccesible.
> - **Anillo persistente** tras clic por usar `:focus` en vez de `:focus-visible`.
> - **Indicador de bajo contraste**: invisible sobre ciertos fondos.

## Notas relacionadas

- [[04 focus-visible]] — el foco solo por teclado (para el anillo).
- [[05 focus-within]] — un ancestro con descendiente enfocado.
- [[01 outline]] — el contorno del anillo de foco.
