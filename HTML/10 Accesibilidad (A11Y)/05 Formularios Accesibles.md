---
title: Formularios Accesibles — Etiquetas, errores y agrupación
aliases:
  - accessible forms
tags:
  - html
  - api/concepto
  - a11y
draft: false
order: 2
---

# Formularios Accesibles

> [!definicion]
> Un formulario accesible se puede completar con teclado y lector de pantalla: cada campo tiene su **etiqueta**, los grupos su **título**, los **errores** se comunican con texto (no solo color), y el **foco** se gestiona al validar. Reúne aquí lo que la [[07 Formularios/index | sección de formularios]] ya aplicó, desde la perspectiva de la accesibilidad.

```html
<form>
  <label for="email">Correo</label>
  <input type="email" id="email" required
         aria-describedby="email-ayuda" aria-invalid="false" />
  <p id="email-ayuda">Usaremos tu correo solo para el pedido.</p>
</form>
```

## Los pilares

| Pilar | Cómo |
|-------|------|
| Etiqueta por campo | Un `<label for>` o envolvente |
| Grupos con título | `<fieldset>` + `<legend>` |
| Instrucciones | `aria-describedby` apuntando al texto de ayuda |
| Errores | Texto + `aria-invalid` + foco al primer error |
| Estados | `required` real, no solo un asterisco visual |

## Etiquetas: lo no negociable

> [!warning] Sin label no hay formulario accesible
> El fallo más común y grave: campos sin etiqueta asociada. Un lector de pantalla, al llegar a un `<input>` sin `<label>`, anuncia "campo de edición" sin decir **qué** pide. El `placeholder` **no** sustituye al label (desaparece, bajo contraste, no siempre se anuncia). Cada control necesita su `<label for>` (o envolverlo). Detalle en [[01 Etiqueta de Campo (label) | `<label>`]].

## Comunicar errores accesiblemente

> [!info] El error necesita texto, no solo color
> Marcar un campo con error solo con un borde rojo excluye a quien no distingue colores y a quien no ve la pantalla. Un error accesible combina:
> 1. **Texto** que describe el problema ("El correo no es válido").
> 2. **`aria-invalid="true"`** en el campo.
> 3. **`aria-describedby`** del campo apuntando al mensaje de error.
> 4. **Mover el foco** al primer error (o a un resumen) al validar.
> ```html
> <label for="cp">Código postal</label>
> <input id="cp" aria-invalid="true" aria-describedby="cp-err" />
> <p id="cp-err">Debe tener 5 dígitos.</p>
> ```
> El resumen de errores al inicio del formulario suele marcarse como [[06 Regiones Vivas (aria-live, aria-atomic, aria-relevant) | región viva]] o recibe el foco para anunciarse.

## Agrupar opciones relacionadas

Los grupos de [[03 Tipos de input/10 input radio | radios]] o checkboxes van en un `<fieldset>` con `<legend>`, para que el lector anuncie la **pregunta** del grupo con cada opción ("Método de pago: Tarjeta, opción 1 de 2").

## Indicar lo obligatorio

`required` debe ser **real** (atributo HTML), no solo un asterisco visual. El asterisco ayuda a quien ve; el atributo `required` (y, si acaso, `aria-required`) lo comunica a la asistencia y activa la validación nativa.

## Buenas prácticas

> [!tip] Recomendaciones
> - Etiqueta **todos** los controles; el placeholder no cuenta.
> - Agrupa opciones con `<fieldset>`/`<legend>`.
> - Comunica errores con texto, `aria-invalid` y `aria-describedby`; mueve el foco al primero.
> - Usa `required` real y señálalo también visualmente.
> - Asocia instrucciones con `aria-describedby`.

## Errores comunes

> [!warning] Trampas
> - **Campos sin `<label>`** o con el placeholder como única etiqueta.
> - **Errores solo en rojo** sin texto ni `aria-invalid`.
> - **Grupos de radios sin `<fieldset>`/`<legend>`**.
> - **No mover el foco** al error tras una validación fallida.

## Notas relacionadas

- [[07 Etiquetas y Agrupación/index]] — label, fieldset, legend (la base).
- [[09 Validación de Formularios/04 Pseudoclases de Validación]] — el estilado accesible de errores.
- [[02 Gestión de Foco (focus, blur)]] — mover el foco al primer error.
