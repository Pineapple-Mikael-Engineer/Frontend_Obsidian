---
title: checked e indeterminate — Casillas y radios marcados
aliases:
  - ":checked"
  - ":indeterminate"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":checked"
grupo: pseudoclase
draft: false
order: 1
---

# :checked e :indeterminate

> [!definicion]
> `:checked` selecciona casillas (`<input type="checkbox">`), radios y `<option>` que están **marcados**. `:indeterminate` selecciona casillas en estado **indeterminado** (ni marcadas ni desmarcadas, típico de un "seleccionar todo" parcial). Son la base de interruptores y controles personalizados sin JavaScript.

```css
input:checked + label { font-weight: bold; color: #cba6f7; }
```

## Estilar según el estado marcado

> [!tip] El hermano del checkbox reacciona
> Como no se puede estilar bien el checkbox nativo, el patrón es **ocultarlo** y estilar un `<label>` o elemento hermano según `:checked`, usando el combinador [[03 Hermano Adyacente (+) | `+`]] o `~`:
> ```css
> .switch input { position: absolute; opacity: 0; }   /* checkbox oculto pero accesible */
> .switch input:checked + .slider { background: #a6e3a1; }   /* el track cambia */
> .switch input:checked + .slider::before { transform: translateX(20px); }   /* el thumb se mueve */
> ```
> Así se construyen **interruptores** (toggles), pestañas y acordeones funcionales solo con CSS.

## indeterminate: ni sí ni no

> [!info] El tercer estado de las casillas
> Una casilla puede estar en **tres** estados: marcada, desmarcada e **indeterminada**. El indeterminado (una rayita en vez de un check) se usa para un "seleccionar todo" cuando **algunos** sub-elementos están marcados pero no todos. Solo se activa por **JavaScript** (`el.indeterminate = true`), no por HTML, pero se puede estilar:
> ```css
> input:indeterminate + label { color: #f9e2af; font-style: italic; }
> ```
> También aplica a `<progress>` sin valor (barra de progreso indeterminada).

## Checkbox hack: componentes sin JS

> [!info] El "checkbox hack" y sus límites
> `:checked` permite construir componentes interactivos **sin JavaScript** (el "checkbox hack"): menús, acordeones, pestañas, modales que se controlan con un checkbox/radio oculto. Funciona, pero como [[06 target | `:target`]] y el truco del [[03 Menús Hamburguesa | menú hamburguesa]], tiene **limitaciones de accesibilidad** (el estado no siempre se comunica bien a la asistencia, gestión de foco pobre). Sirve para casos simples; para componentes serios, JS con ARIA.

## Checkbox y radio personalizados

```css
/* Ocultar el control nativo y dibujar uno propio */
input[type="checkbox"] { appearance: none; width: 1.2em; height: 1.2em; border: 2px solid #ccc; border-radius: 4px; }
input[type="checkbox"]:checked { background: #cba6f7; border-color: #cba6f7; }
input[type="checkbox"]:checked::after { content: "✓"; color: white; }
```

> [!tip] accent-color, la forma simple
> Para solo cambiar el **color** de los controles nativos (sin rediseñarlos), `accent-color` es mucho más simple que el checkbox hack:
> ```css
> input[type="checkbox"], input[type="radio"] { accent-color: #cba6f7; }
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Para personalizar controles, oculta el nativo (manteniéndolo accesible) y estila un hermano con `:checked`.
> - Usa `accent-color` si solo quieres cambiar el color del control nativo.
> - `:indeterminate` para "seleccionar todo" parcial (se activa por JS).
> - El "checkbox hack" sirve para casos simples; usa JS+ARIA para componentes serios.

## Errores comunes

> [!warning] Trampas
> - **Ocultar el checkbox con `display: none`**: lo saca del foco/accesibilidad (usa `opacity: 0` + posición).
> - **Componentes con checkbox hack** que fallan en accesibilidad.
> - **Olvidar `:indeterminate`** se activa solo por JS, no por HTML.

## Notas relacionadas

- [[03 Hermano Adyacente (+)]] — el combinador para estilar el hermano.
- [[06 Personalización de Controles (accent-color, appearance)]] — `accent-color` y `appearance`.
- [[03 Menús Hamburguesa]] — el checkbox hack y sus límites.
