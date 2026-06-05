---
title: Contenido Oculto Visualmente (sr-only) — Texto solo para lectores
aliases:
  - sr-only
  - visually hidden
tags:
  - html
  - api/concepto
  - a11y
draft: false
---

# Contenido Oculto Visualmente (sr-only)

> [!definicion]
> A veces se necesita texto que **los lectores de pantalla lean** pero que **no se vea** en pantalla: un nombre para un control que visualmente se entiende por contexto, una aclaración, una etiqueta. La técnica `sr-only` (screen-reader only) oculta el texto visualmente **sin** quitarlo del árbol de accesibilidad.

```html
<a href="/carrito">
  <svg aria-hidden="true">🛒</svg>
  <span class="sr-only">Ver carrito de la compra</span>
</a>
```

## La clase sr-only

No es un atributo nativo, sino una **clase CSS** convencional que esconde el elemento de la vista manteniéndolo accesible:

```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

## Por qué no display: none ni visibility: hidden

> [!warning] Ocultar mal lo quita también de la asistencia
> Las formas "obvias" de ocultar **también** lo eliminan para los lectores de pantalla, que es justo lo contrario de lo buscado:
> | Técnica | ¿Visible? | ¿Lo leen los lectores? |
> |---------|-----------|------------------------|
> | `display: none` | No | **No** (eliminado) |
> | `visibility: hidden` | No | **No** (eliminado) |
> | `sr-only` (clip/posición) | No | **Sí** ✅ |
> | `aria-hidden="true"` | Sí | No |
>
> `sr-only` y `aria-hidden` son **opuestos**: `sr-only` oculta a la vista pero no a la asistencia; `aria-hidden` oculta a la asistencia pero no a la vista.

## Casos de uso

| Caso | Ejemplo |
|------|---------|
| Nombre de un control de solo icono | "Ver carrito" tras un icono |
| Aclarar un enlace ambiguo | "Leer más **sobre la tarta**" (el "sobre la tarta" oculto) |
| Etiqueta de un campo sin label visible | Un buscador con lupa |
| Texto del skip link | "Saltar al contenido" |
| Encabezado de sección no visual | Estructurar para lectores |

## sr-only vs. aria-label

Ambos dan un nombre accesible sin texto visible. Diferencias:

- **`sr-only`**: texto **real** en el DOM (traducible por el navegador, seleccionable por herramientas). Más robusto.
- **`aria-label`**: una cadena en un atributo (no siempre traducida por los traductores automáticos del navegador).

Para nombres importantes, `sr-only` con texto real suele ser preferible; `aria-label` es más conciso para casos simples.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `sr-only` cuando necesites texto leído pero no visto, con una técnica que lo mantenga accesible.
> - **Nunca** ocultes con `display: none`/`visibility: hidden` lo que deba leer la asistencia.
> - Prefiere texto real (`sr-only`) a `aria-label` cuando la traducción importe.
> - No abuses: si el texto ayuda a **todos**, muéstralo en vez de ocultarlo.

## Errores comunes

> [!warning] Trampas
> - **`display: none`** creyendo que solo oculta visualmente: también lo quita de la asistencia.
> - **Confundir `sr-only` con `aria-hidden`**: hacen lo opuesto.
> - **Ocultar contenido que debería ser visible** para todos.

## Notas relacionadas

- [[02 Texto para Iconos]] — `sr-only` para nombrar botones de icono.
- [[05 Estados ARIA (expanded, hidden, checked, selected)]] — `aria-hidden`, su opuesto.
- [[03 Skip Links]] — usa esta técnica para ocultarse hasta recibir foco.
