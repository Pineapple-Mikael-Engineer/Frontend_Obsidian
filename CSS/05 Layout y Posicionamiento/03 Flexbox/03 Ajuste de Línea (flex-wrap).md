---
title: flex-wrap — Permitir varias líneas
aliases:
  - flex-wrap
tags:
  - css
  - api/propiedad
  - layout
propiedad: flex-wrap
grupo: flexbox
valor_inicial: nowrap
hereda: false
animable: false
draft: false
order: 3
---

# flex-wrap

> [!definicion]
> `flex-wrap` decide si los items flex que no caben **se ajustan a varias líneas** o se comprimen en una sola. Por defecto (`nowrap`), todos van en una línea aunque se desborden; `wrap` permite que salten a la siguiente línea cuando falta espacio.

```css
.galeria {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}
```

## Valores

| Valor | Comportamiento |
|-------|----------------|
| `nowrap` | Todos en una línea; se encogen o desbordan (por defecto) |
| `wrap` | Saltan a líneas nuevas cuando no caben |
| `wrap-reverse` | Igual que wrap, pero las líneas se apilan al revés |

## El problema de nowrap

> [!warning] Por defecto, los items no se ajustan
> Con `nowrap` (el valor inicial), si los items no caben, el navegador intenta **encogerlos** ([[09 flex-grow, flex-shrink, flex-basis | `flex-shrink`]]) y, si no pueden encoger más, **desbordan** el contenedor. Por eso una fila de tarjetas sin `flex-wrap: wrap` se aprieta o se sale en pantallas pequeñas:
> ```css
> .tarjetas { display: flex; flex-wrap: wrap; }   /* se reorganizan en varias filas */
> ```

## Layout responsivo sin media queries

> [!tip] flex-wrap + flex-basis = rejilla fluida
> Combinando `flex-wrap: wrap` con un `flex-basis` (ancho ideal) y `flex-grow`, los items forman una rejilla que se **reorganiza sola** según el ancho, sin media queries:
> ```css
> .galeria { display: flex; flex-wrap: wrap; gap: 1rem; }
> .galeria > * {
>   flex: 1 1 15rem;   /* crece, encoge, ideal 15rem */
> }
> ```
> Cada item intenta medir 15rem; cuando no caben, saltan de línea; el espacio sobrante se reparte. Es la "rejilla flexible" clásica (aunque [[02 Grid con auto-fit | Grid con `auto-fit`]] da más control).

## El shorthand flex-flow

`flex-flow` combina `flex-direction` y `flex-wrap` en una propiedad:

```css
flex-flow: row wrap;       /* = flex-direction: row; flex-wrap: wrap; */
flex-flow: column nowrap;
```

## Cómo se reparte el espacio entre líneas

Cuando hay varias líneas (wrap activo), [[06 Líneas Múltiples (align-content) | `align-content`]] controla cómo se distribuyen esas líneas en el eje transversal (separadas, agrupadas, centradas). Sin wrap, `align-content` no tiene efecto (solo hay una línea).

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `flex-wrap: wrap` en galerías, listas de etiquetas y tarjetas para que se reorganicen.
> - Combínalo con `flex: 1 1 <ancho>` para rejillas fluidas sin media queries.
> - Para rejillas con más control (filas alineadas, columnas iguales), valora Grid.
> - `flex-flow` para abreviar dirección + wrap.

## Errores comunes

> [!warning] Trampas
> - **Olvidar `wrap`**: los items se aprietan o desbordan en pantallas pequeñas.
> - **Esperar que `align-content` funcione** sin wrap (solo aplica con varias líneas).
> - **Filas desiguales** con flex wrap: si necesitas columnas perfectamente alineadas, usa Grid.

## Notas relacionadas

- [[09 flex-grow, flex-shrink, flex-basis]] — el `flex: 1 1 <ancho>` de las rejillas fluidas.
- [[06 Líneas Múltiples (align-content)]] — alinear las líneas cuando hay wrap.
- [[12 Patrones (auto-fit, auto-fill)]] — la alternativa con Grid.
