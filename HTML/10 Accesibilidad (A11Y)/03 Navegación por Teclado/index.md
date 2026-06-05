---
title: Navegación por Teclado — Usar la web sin ratón
aliases:
  - keyboard navigation
tags:
  - html
  - api/concepto
  - a11y
draft: false
---

# Navegación por Teclado

> [!definicion]
> Muchas personas navegan **solo con teclado**: usuarios de lector de pantalla, personas con dificultades motoras, usuarios avanzados. Todo lo que se pueda hacer con ratón debe poder hacerse con teclado: avanzar con `Tab`, activar con `Enter`/`Espacio`, sin quedar atrapado. Es uno de los pilares de la accesibilidad (WCAG: "Operable").

```html
<!-- Enfocable y operable por teclado por defecto -->
<a href="/">Inicio</a>
<button>Enviar</button>
<input type="text" />
```

## El orden de tabulación

Al pulsar `Tab`, el foco recorre los elementos **interactivos** (enlaces, botones, campos) en el **orden del DOM**. Por eso un orden de marcado lógico produce una navegación por teclado lógica, sin esfuerzo extra.

| Tecla | Acción |
|-------|--------|
| `Tab` | Siguiente elemento enfocable |
| `Shift + Tab` | Anterior |
| `Enter` | Activar enlaces y botones |
| `Espacio` | Activar botones, marcar casillas |
| `Flechas` | Moverse dentro de un grupo (radios, menús, sliders) |
| `Esc` | Cerrar diálogos/menús |

## Mapa de la sección

- [[01 tabindex]] — controlar qué entra en el orden de tabulación.
- [[02 Gestión de Foco (focus, blur)]] — mover y gestionar el foco con JavaScript.
- [[03 Skip Links]] — saltar la navegación repetida e ir al contenido.
- [[04 Trampas de Foco (focus trapping)]] — atrapar el foco en un modal (a propósito) y evitar atraparlo por error.

## El foco debe verse

> [!warning] No elimines el indicador de foco
> El **anillo de foco** (el borde que marca dónde está el foco) es imprescindible para quien navega con teclado: sin él, no sabe dónde está. El antipatrón clásico es `outline: none` en CSS para "limpiar" el diseño, dejando la web inutilizable por teclado. Si el outline por defecto no encaja, **reemplázalo** por uno propio visible, nunca lo elimines:
> ```css
> :focus-visible { outline: 2px solid #cba6f7; outline-offset: 2px; }
> ```
> `:focus-visible` muestra el anillo solo en navegación por teclado, no al hacer clic con ratón.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa elementos nativos (enlaces, botones): ya son enfocables y operables.
> - Mantén un orden de DOM lógico (= orden de tabulación lógico).
> - Conserva un indicador de foco visible (reemplázalo, no lo elimines).
> - Prueba la página entera **solo con teclado**: ¿se puede hacer todo?

## Notas relacionadas

- [[01 tabindex]] — el control fino del orden de foco.
- [[01 HTML Semántico como Base]] — los elementos nativos ya son operables por teclado.
