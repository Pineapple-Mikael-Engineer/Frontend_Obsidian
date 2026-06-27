---
title: Roles de Widget — Componentes interactivos a medida
aliases:
  - widget roles
tags:
  - html
  - api/concepto
  - a11y
draft: false
order: 3
---

# Roles de Widget

> [!definicion]
> Los **roles de widget** describen **componentes interactivos** que el HTML no tiene de forma nativa: pestañas, menús, diálogos modales, carruseles, árboles. Aquí es donde ARIA es **realmente necesaria**, pero también donde más responsabilidad asume el desarrollador: el rol no aporta comportamiento, hay que programar el teclado.

```html
<div role="tablist" aria-label="Configuración">
  <button role="tab" aria-selected="true" id="t1" aria-controls="p1">General</button>
  <button role="tab" aria-selected="false" id="t2" aria-controls="p2">Avanzado</button>
</div>
<div role="tabpanel" id="p1" aria-labelledby="t1">…</div>
```

## Roles de widget comunes

| Rol | Componente |
|-----|------------|
| `tab` / `tablist` / `tabpanel` | Pestañas |
| `dialog` / `alertdialog` | Ventana modal |
| `menu` / `menuitem` | Menú de aplicación |
| `tooltip` | Información emergente |
| `tree` / `treeitem` | Árbol jerárquico |
| `slider` | Deslizador a medida |
| `switch` | Interruptor on/off |

## El rol no da comportamiento

> [!warning] ARIA describe, no implementa
> Poner `role="tab"` hace que el lector de pantalla **anuncie** "pestaña", pero **no** añade ninguna funcionalidad. Un widget de pestañas accesible exige, además del rol, programar con JavaScript:
> - **Navegación con teclado** (flechas entre pestañas, Tab para salir).
> - **Gestión del foco** ([[01 tabindex | `tabindex`]] roving).
> - **Estados** (`aria-selected` que cambia al activar).
>
> Implementar bien un widget ARIA es complejo; por eso conviene seguir patrones probados en lugar de improvisar.

## Sigue los Authoring Practices

> [!tip] No reinventes el patrón
> El **WAI-ARIA Authoring Practices Guide (APG)** documenta cada widget (pestañas, combobox, diálogo…) con su marcado, sus roles, sus estados y el **teclado exacto** que debe responder. Antes de construir un componente complejo desde cero, consúltalo: ahorra errores y garantiza que el teclado cumple lo que los usuarios esperan.

## Antes de un widget, ¿hay HTML nativo?

Muchos "widgets" tienen un equivalente nativo más simple y accesible:

| En vez del widget ARIA | Considera |
|------------------------|-----------|
| `role="dialog"` a medida | El elemento `<dialog>` nativo |
| `role="switch"` | Un `<input type="checkbox">` estilado |
| Acordeón a medida | `<details>`/`<summary>` |
| `role="button"` | Un `<button>` |

## Buenas prácticas

> [!tip] Recomendaciones
> - Antes de un widget ARIA, comprueba si hay un elemento nativo (`<dialog>`, `<details>`).
> - Si construyes el widget, implementa el **teclado completo** según el APG.
> - Mantén los estados (`aria-selected`, `aria-expanded`) sincronizados con JS.
> - Prueba con un lector de pantalla real, no solo visualmente.

## Errores comunes

> [!warning] Trampas
> - **Rol sin teclado**: el widget "suena" bien pero no se puede manejar.
> - **Reinventar** un componente que el HTML ya ofrece.
> - **Estados que no cambian**: `aria-selected` fijo aunque cambie la pestaña.

## Notas relacionadas

- [[01 tabindex]] · [[02 Gestión de Foco (focus, blur)]] — el teclado que los widgets exigen.
- [[05 Estados ARIA (expanded, hidden, checked, selected)]] — los estados de los widgets.
- [[index]] — la regla de "agotar el HTML nativo primero".
