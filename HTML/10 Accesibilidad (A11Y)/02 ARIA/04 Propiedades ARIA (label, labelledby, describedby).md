---
title: Propiedades ARIA — label, labelledby, describedby
aliases:
  - aria-label
  - aria-labelledby
  - aria-describedby
tags:
  - html
  - api/concepto
  - a11y
draft: false
---

# Propiedades ARIA (label, labelledby, describedby)

> [!definicion]
> Estas tres propiedades dan a un elemento su **nombre** y su **descripción** accesibles cuando el contenido visible no basta: un control de solo icono, un grupo sin título, un campo con instrucciones. Son de las propiedades ARIA más útiles y usadas.

```html
<button aria-label="Cerrar">✕</button>

<nav aria-labelledby="titulo-nav">
  <h2 id="titulo-nav">Navegación principal</h2>
</nav>

<input aria-describedby="ayuda-clave" />
<p id="ayuda-clave">Mínimo 8 caracteres.</p>
```

## Las tres y su función

| Propiedad | Qué aporta | Cómo |
|-----------|------------|------|
| `aria-label` | El **nombre**, como texto directo | Una cadena en el atributo |
| `aria-labelledby` | El **nombre**, referenciando otro elemento | El `id` de un elemento visible |
| `aria-describedby` | Una **descripción** adicional | El `id` de un elemento con el texto |

## label vs. labelledby

> [!info] Texto propio vs. referencia
> - **`aria-label="Cerrar"`**: pones el texto directamente. Útil cuando no hay texto visible que reutilizar (un botón de icono).
> - **`aria-labelledby="id"`**: apuntas a un texto **que ya existe** en la página. Útil para reutilizar un encabezado como nombre del landmark, o combinar varios textos.
>
> Si hay un texto visible que sirve de etiqueta, `aria-labelledby` (referenciarlo) es mejor que duplicarlo en `aria-label`.

## describedby: la descripción extra

`aria-describedby` añade información **complementaria** que se anuncia después del nombre: instrucciones de un campo, el formato esperado, un mensaje de error. No reemplaza la etiqueta; la complementa:

```html
<label for="clave">Contraseña</label>
<input id="clave" type="password" aria-describedby="reglas error" />
<p id="reglas">Mínimo 8 caracteres, una mayúscula.</p>
<p id="error">La contraseña es demasiado corta.</p>
```

`aria-describedby` puede referenciar **varios** `id` separados por espacios.

## El label nativo es preferible

> [!warning] Para formularios, primero <label>
> En formularios, el elemento [[01 Etiqueta de Campo (label) | `<label>`]] nativo es **mejor** que `aria-label`: además del nombre accesible, amplía el área clicable y es visible para todos. `aria-label`/`aria-labelledby` se reservan para cuando no hay un `<label>` posible (un control de icono, un buscador sin etiqueta visible). No sustituyas un `<label>` por `aria-label` sin motivo.

## Buenas prácticas

> [!tip] Recomendaciones
> - Da nombre accesible a **todo** control de solo icono con `aria-label`.
> - Prefiere `aria-labelledby` cuando ya existe un texto visible que sirva de etiqueta.
> - Usa `aria-describedby` para instrucciones, formatos y errores de los campos.
> - En formularios, el `<label>` nativo va primero; ARIA solo cuando no encaje.

## Errores comunes

> [!warning] Trampas
> - **`aria-label` que tapa** el texto visible (el lector anuncia el label, no el contenido).
> - **`labelledby`/`describedby` con `id` inexistente**: no aporta nada.
> - **Sustituir `<label>`** por `aria-label` sin necesidad: pierdes el área clicable.

## Notas relacionadas

- [[01 Etiqueta de Campo (label)]] — la etiqueta nativa, preferible en formularios.
- [[05 Estados ARIA (expanded, hidden, checked, selected)]] — los estados, su complemento dinámico.
- [[02 Texto para Iconos]] — nombrar controles de solo icono.
