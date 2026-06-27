---
title: Estados ARIA — expanded, hidden, checked, selected
aliases:
  - aria states
  - aria-expanded
tags:
  - html
  - api/concepto
  - a11y
draft: false
order: 5
---

# Estados ARIA (expanded, hidden, checked, selected)

> [!definicion]
> Los **estados ARIA** comunican la **situación actual** de un elemento interactivo, que cambia con la interacción: si un menú está desplegado, si una casilla está marcada, si una pestaña está activa. A diferencia de las propiedades (estables), los estados **se actualizan** con JavaScript a medida que el usuario interactúa.

```html
<button aria-expanded="false" aria-controls="menu">Menú</button>
<ul id="menu" hidden>…</ul>
```

```js
boton.addEventListener("click", () => {
  const abierto = boton.getAttribute("aria-expanded") === "true";
  boton.setAttribute("aria-expanded", String(!abierto));
  menu.hidden = abierto;
});
```

## Estados frecuentes

| Estado | Comunica |
|--------|----------|
| `aria-expanded` | Si un elemento desplegable está abierto (`true`/`false`) |
| `aria-checked` | Estado de una casilla/switch a medida (`true`/`false`/`mixed`) |
| `aria-selected` | Si una opción/pestaña está seleccionada |
| `aria-hidden` | Si un elemento se oculta a la asistencia (`true`) |
| `aria-disabled` | Si está deshabilitado (sin quitarle el foco) |
| `aria-pressed` | Estado de un botón de alternancia (toggle) |
| `aria-current` | El elemento actual de un conjunto (`page`, `step`) |

## La regla de oro: sincronizar con la realidad

> [!warning] Un estado mentiroso es peor que ninguno
> El error más grave con los estados ARIA es que **no reflejen la realidad**: un `aria-expanded="false"` en un menú que está abierto le dice al usuario de lector de pantalla justo lo contrario de lo que ocurre. Cada vez que el estado real cambia (al hacer clic, al abrir), hay que **actualizar el atributo con JavaScript**. Un estado estático que no cambia es una fuente activa de confusión.

## aria-hidden: ocultar a la asistencia

`aria-hidden="true"` quita un elemento del árbol de accesibilidad (el lector lo ignora), aunque siga visible. Se usa para iconos decorativos o contenido duplicado:

```html
<button>
  <svg aria-hidden="true">…</svg>   <!-- icono decorativo, ignorado -->
  Guardar
</button>
```

> [!warning] No ocultes contenido interactivo
> `aria-hidden="true"` en un elemento que **contiene controles enfocables** (un botón, un enlace) crea una incoherencia: el usuario puede llegar con Tab a un control que el lector "no ve". Nunca pongas `aria-hidden` sobre algo enfocable.

## checked nativo vs. aria-checked

Para una casilla normal, [[09 input checkbox | `<input type="checkbox">`]] gestiona su estado solo. `aria-checked` es para casillas/switches **a medida** (`role="checkbox"`/`role="switch"`), donde hay que mantenerlo a mano. De nuevo: el nativo, cuando existe, evita este trabajo.

## Buenas prácticas

> [!tip] Recomendaciones
> - **Actualiza** el estado con JS cada vez que cambie la realidad.
> - Usa `aria-expanded` en todo disparador de desplegables (menús, acordeones).
> - `aria-hidden="true"` para iconos decorativos; nunca sobre contenido enfocable.
> - Para controles con equivalente nativo, usa el nativo y olvídate del estado ARIA.

## Errores comunes

> [!warning] Trampas
> - **Estado desincronizado** con la realidad.
> - **`aria-hidden` sobre elementos enfocables**: el foco entra en lo "oculto".
> - **`aria-checked` a mano** cuando un `<input>` nativo lo haría solo.

## Notas relacionadas

- [[04 Propiedades ARIA (label, labelledby, describedby)]] — las propiedades estables, su complemento.
- [[03 Roles de Widget]] — los widgets que dependen de estos estados.
- [[05 Ocultar (hidden)]] — el atributo `hidden`, que oculta a todos (no solo a la asistencia).
