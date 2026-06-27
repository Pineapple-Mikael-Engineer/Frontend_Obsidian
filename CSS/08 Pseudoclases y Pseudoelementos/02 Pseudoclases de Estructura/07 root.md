---
title: root — La raíz del documento
aliases:
  - ":root"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":root"
grupo: pseudoclase
draft: false
order: 7
---

# :root

> [!definicion]
> `:root` selecciona el elemento **raíz** del documento: en HTML, siempre el `<html>`. Su uso fundamental es declarar las **variables CSS globales** (custom properties), porque define el ámbito más alto del que todo el documento hereda.

```css
:root {
  --color-primario: #cba6f7;
  --espaciado: 1rem;
}
```

## El hogar de las variables globales

> [!tip] :root es donde viven las custom properties globales
> La razón principal de `:root`: declarar las [[10 Variables CSS/index | variables CSS]] que deben estar disponibles en **todo** el documento. Como cada elemento hereda de la raíz, las variables definidas en `:root` son accesibles desde cualquier parte:
> ```css
> :root {
>   --color-texto: #1e1e2e;
>   --color-fondo: #ffffff;
>   --fuente-base: system-ui, sans-serif;
>   --radio: 8px;
> }
> body { color: var(--color-texto); font-family: var(--fuente-base); }
> .card { border-radius: var(--radio); }
> ```
> Es la convención universal para el "tema" base de un sitio.

## :root vs. html

> [!info] Casi lo mismo, pero :root tiene más especificidad
> `:root` y `html` seleccionan el mismo elemento, con una diferencia sutil:
> - `:root` es una **pseudoclase**, así que tiene mayor [[01 Especificidad/index | especificidad]] (0,1,0) que el selector de tipo `html` (0,0,1).
> - Por convención, `:root` se usa para variables y configuración global; `html` para estilos de elemento (tamaño de fuente base, etc.).
>
> Para las variables, **siempre `:root`** (es la convención reconocible y tiene la especificidad adecuada).

## Modo oscuro con :root

> [!tip] Redefinir variables en :root para temas
> `:root` es donde se cambian las variables al alternar de tema (modo oscuro), redefiniéndolas en una media query o con una clase:
> ```css
> :root { --fondo: #fff; --texto: #1e1e2e; }
> @media (prefers-color-scheme: dark) {
>   :root { --fondo: #1e1e2e; --texto: #cdd6f4; }
> }
> ```
> Con solo cambiar las variables en `:root`, todo el sitio adopta el tema. Detalle en [[06 Temas Dinámicos]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Declara las variables CSS globales en `:root` (la convención universal).
> - Usa `:root` (no `html`) para variables y configuración global.
> - Redefine variables en `:root` dentro de media queries para temas (modo oscuro).
> - Reserva `html` para estilos de elemento como el `font-size` base.

## Errores comunes

> [!warning] Trampas
> - **Variables en `body`** en vez de `:root`: funcionan, pero `:root` es la convención y cubre todo.
> - **Confundir la especificidad** de `:root` (0,1,0) con `html` (0,0,1).

## Notas relacionadas

- [[10 Variables CSS/index]] — las custom properties que viven en `:root`.
- [[06 Temas Dinámicos]] — cambiar el tema redefiniendo variables.
- [[01 Especificidad/index]] — por qué `:root` pesa más que `html`.
