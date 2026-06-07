---
title: valid e invalid — Validación del valor
aliases:
  - ":valid"
  - ":invalid"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":valid"
grupo: pseudoclase
draft: false
---

# :valid e :invalid

> [!definicion]
> `:valid` selecciona los campos cuyo valor **cumple** las reglas de validación del HTML (tipo, `required`, `pattern`, `min`/`max`); `:invalid`, los que **no** las cumplen. Permiten feedback de validación instantáneo sin JavaScript, reaccionando a la validación nativa del navegador.

```css
input:invalid { border-color: #f38ba8; }
input:valid { border-color: #a6e3a1; }
```

## Validación nativa, reacción en CSS

> [!info] El navegador valida según el HTML
> El navegador valida automáticamente según los atributos del campo: un `type="email"` es `:invalid` si el texto no parece un email; un `required` vacío es `:invalid`; un `pattern` que no casa es `:invalid`. CSS solo **reacciona**:
> ```css
> input[type="email"]:invalid { border-color: #f38ba8; }
> input[type="email"]:valid { border-color: #a6e3a1; }
> ```
> Detalle de los atributos en [[03 Validación HTML5/index]].

## El gran problema: :invalid prematuro

> [!warning] :invalid se activa antes de escribir
> El defecto más conocido: `:invalid` se aplica **desde que carga la página**, antes de que el usuario toque nada. Un campo `required` vacío aparece **en rojo de entrada**, lo que es agresivo y confuso ("¿por qué me regaña si no he escrito?"):
> ```css
> /* ❌ marca en rojo los campos vacíos al cargar */
> input:invalid { border-color: red; }
> ```
> La solución moderna es [[07 user-valid y user-invalid | `:user-invalid`]], que solo se activa **tras** la interacción. Hasta que tuvo soporte, se usaban trucos como combinar con `:not(:placeholder-shown)` o `:focus`.

## Los apaños tradicionales

> [!tip] Limitar :invalid hasta que haya interacción
> Antes de `:user-invalid`, el truco para no marcar campos vacíos prematuramente era combinar `:invalid` con otras condiciones:
> ```css
> /* Solo marcar en rojo si el campo no está vacío y es inválido */
> input:not(:placeholder-shown):invalid { border-color: #f38ba8; }
> /* O solo al perder el foco (con :focus) */
> input:invalid:not(:focus) { border-color: #f38ba8; }
> ```
> `:not(:placeholder-shown)` requiere que el campo tenga un `placeholder` y detecta si el usuario ha escrito algo. Hoy, `:user-invalid` es más limpio.

## Feedback con :has() en el grupo

> [!tip] Resaltar el grupo entero del campo inválido
> Con [[03 has() | `:has()`]], se resalta el grupo completo (label, mensaje de error) según la validez:
> ```css
> .campo:has(input:invalid) label { color: #f38ba8; }
> .campo:has(input:user-invalid) .mensaje-error { display: block; }
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Prefiere [[07 user-valid y user-invalid | `:user-invalid`]] a `:invalid` para evitar el rojo prematuro.
> - Si usas `:invalid`, combínalo con `:not(:placeholder-shown)` o `:not(:focus)`.
> - No marques la validación solo con color: añade iconos/mensajes.
> - Combina con `:has()` para resaltar el grupo y mostrar mensajes de error.

## Errores comunes

> [!warning] Trampas
> - **Rojo prematuro**: `:invalid` marca campos vacíos al cargar.
> - **Validación solo por color**: inaccesible.
> - **No dar mensaje** de qué está mal: el borde rojo no explica el error.

## Notas relacionadas

- [[07 user-valid y user-invalid]] — la versión que espera a la interacción (recomendada).
- [[06 in-range y out-of-range]] — validación de rangos numéricos.
- [[03 Validación HTML5/index]] — los atributos que disparan la validación.
