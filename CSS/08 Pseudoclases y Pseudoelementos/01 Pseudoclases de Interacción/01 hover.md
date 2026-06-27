---
title: hover — El ratón por encima
aliases:
  - ":hover"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":hover"
grupo: pseudoclase
draft: false
order: 1
---

# :hover

> [!definicion]
> `:hover` selecciona un elemento mientras el **puntero está encima** de él. Es la pseudoclase más usada para dar feedback visual: botones que cambian de color, enlaces que se subrayan, tarjetas que se elevan al pasar el ratón.

```css
.boton:hover { background: #b48ef0; }
```

## El uso básico

```css
a:hover { text-decoration: underline; }
.card:hover { transform: translateY(-4px); box-shadow: 0 8px 16px rgb(0 0 0 / 0.15); }
.btn:hover { background: #b48ef0; }
```

## No existe hover en táctil

> [!warning] El hover "pegajoso" en móvil
> En dispositivos **táctiles** no hay un cursor que "pase por encima", así que `:hover` se comporta de forma rara: al tocar un elemento, el estado hover **se queda pegado** hasta que se toca otra cosa. Esto puede dejar botones con su color de hover tras pulsarlos. La solución es envolver los efectos de hover en una media query que detecte si el dispositivo **puede** hacer hover:
> ```css
> @media (hover: hover) {
>   .card:hover { transform: translateY(-4px); }
> }
> ```
> Así el efecto solo se aplica donde el hover tiene sentido (ratón, lápiz). Detalle en [[05 Interacción (hover, pointer)]].

## No dependas solo del hover

> [!warning] El hover no es accesible por sí solo
> Información o acciones que **solo** aparecen en `:hover` (un menú que se despliega, un botón que se revela) son **inaccesibles** para quien usa teclado o táctil (no pueden "hacer hover"). El hover debe ser una **mejora**, no la única vía:
> - Acompaña el hover con `:focus`/`:focus-within` para el teclado.
> - No escondas funcionalidad esencial tras el hover.
> ```css
> .menu:hover .submenu, .menu:focus-within .submenu { display: block; }
> ```

## Transiciones suaves

`:hover` se combina casi siempre con [[01 Transiciones (transition)/index | transiciones]] para que el cambio sea fluido. Recuerda declarar la transición en el **estado base**, no en `:hover`:

```css
.btn { transition: background 0.2s; }   /* en el base: suave al entrar Y al salir */
.btn:hover { background: #b48ef0; }
```

## hover en elementos padre

`:hover` se activa también cuando el ratón está sobre los **hijos** del elemento. Esto permite efectos donde pasar por una tarjeta afecta a sus partes:

```css
.card:hover .titulo { color: #cba6f7; }   /* el título cambia al pasar por la tarjeta */
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Envuelve los efectos de hover en `@media (hover: hover)` para evitar el hover pegajoso en táctil.
> - Acompaña el hover con `:focus`/`:focus-within` para accesibilidad por teclado.
> - No escondas funcionalidad esencial solo tras el hover.
> - Usa transiciones (declaradas en el estado base) para suavizar.

## Errores comunes

> [!warning] Trampas
> - **Hover pegajoso** en móvil sin `@media (hover: hover)`.
> - **Funcionalidad solo en hover**: inaccesible por teclado/táctil.
> - **Transición en `:hover`** en vez del estado base: salida brusca.

## Notas relacionadas

- [[03 focus]] · [[05 focus-within]] — el equivalente accesible para teclado.
- [[05 Interacción (hover, pointer)]] — detectar la capacidad de hover.
- [[01 Transiciones (transition)/index]] — suavizar el cambio.
