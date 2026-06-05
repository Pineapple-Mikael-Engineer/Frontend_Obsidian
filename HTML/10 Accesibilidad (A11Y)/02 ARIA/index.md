---
title: ARIA — Accesibilidad para lo que el HTML no cubre
aliases:
  - aria
  - wai-aria
tags:
  - html
  - api/concepto
  - a11y
draft: false
---

# ARIA

> [!definicion]
> **ARIA** (Accessible Rich Internet Applications) es un conjunto de atributos que añaden información de accesibilidad cuando el HTML nativo no llega: componentes interactivos complejos (pestañas, menús, diálogos) que no tienen un elemento propio. Se compone de **roles** (qué es un elemento), **propiedades** (características) y **estados** (situación actual).

```html
<div role="tablist">
  <button role="tab" aria-selected="true" aria-controls="panel1">Pestaña 1</button>
</div>
<div role="tabpanel" id="panel1">…</div>
```

## Las tres categorías de ARIA

| Categoría | Qué define | Ejemplo |
|-----------|------------|---------|
| **Roles** | Qué **es** un elemento | `role="navigation"`, `role="tab"` |
| **Propiedades** | Características estables | `aria-label`, `aria-describedby` |
| **Estados** | Situación actual (cambia) | `aria-expanded`, `aria-checked` |

## Mapa de la sección

- [[01 Roles de Landmark]] — regiones de página (`navigation`, `main`, `banner`…).
- [[02 Roles de Estructura]] — estructura del contenido (`list`, `heading`, `article`…).
- [[03 Roles de Widget]] — componentes interactivos (`tab`, `dialog`, `menu`…).
- [[04 Propiedades ARIA (label, labelledby, describedby)]] — nombrar y describir.
- [[05 Estados ARIA (expanded, hidden, checked, selected)]] — el estado dinámico.
- [[06 Regiones Vivas (aria-live, aria-atomic, aria-relevant)]] — anunciar cambios.

## La primera regla de ARIA: no usar ARIA

> [!warning] ARIA es el último recurso, no el primero
> El propio estándar lo dice: **si un elemento HTML nativo ya ofrece el rol y el comportamiento, úsalo en lugar de ARIA**. Un `<nav>` es mejor que `<div role="navigation">`; un `<button>` es mejor que `<div role="button">`. ARIA **no añade comportamiento** (ni foco ni teclado): solo cambia cómo se anuncia el elemento. Un `role="button"` en un `<div>` lo hace *sonar* a botón, pero sigue sin recibir foco ni responder a Enter salvo que lo programes. ARIA mal usada es **peor** que no usarla.

## Cuándo sí usar ARIA

| Situación | ARIA aporta |
|-----------|-------------|
| Componente sin equivalente HTML (pestañas, carrusel) | Roles y estados de widget |
| Dar nombre a un control de solo icono | `aria-label` |
| Anunciar cambios dinámicos (notificaciones) | `aria-live` |
| Indicar estado (desplegado, seleccionado) | `aria-expanded`, `aria-selected` |

## Buenas prácticas

> [!tip] Recomendaciones
> - Agota primero el HTML nativo; recurre a ARIA solo para lo que no cubra.
> - Si usas un rol de widget, **implementa también el teclado** (ARIA no lo da).
> - Mantén los estados ARIA sincronizados con la realidad (actualízalos con JS).
> - Sigue los patrones del **WAI-ARIA Authoring Practices** para componentes complejos.

## Errores comunes

> [!warning] Trampas
> - **ARIA para recrear** elementos nativos: más trabajo, peor resultado.
> - **Rol sin teclado**: `role="button"` en un `<div>` sin manejar Enter/Espacio.
> - **Estados desactualizados**: `aria-expanded="false"` cuando ya está abierto.
> - **ARIA redundante**: `<button role="button">` (el botón ya es botón).

## Notas relacionadas

- [[01 HTML Semántico como Base]] — lo que se debe agotar antes de ARIA.
- [[03 Roles de Widget]] — el caso donde ARIA es realmente necesaria.
