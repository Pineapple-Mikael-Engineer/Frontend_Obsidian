---
title: in-range y out-of-range — Valores numéricos dentro o fuera de rango
aliases:
  - ":in-range"
  - ":out-of-range"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":in-range"
grupo: pseudoclase
draft: false
---

# :in-range y :out-of-range

> [!definicion]
> `:in-range` selecciona los campos numéricos cuyo valor está **dentro** de los límites `min`/`max`; `:out-of-range`, los que están **fuera**. Solo aplican a campos con rango definido (`type="number"`, `range`, `date`…) y dan feedback específico sobre límites.

```css
input:out-of-range { border-color: #f38ba8; background: #fff5f5; }
```

## Solo con min/max

> [!info] Requieren un rango definido
> Estas pseudoclases solo tienen sentido en campos que aceptan un **rango** y tienen `min` y/o `max`:
> ```html
> <input type="number" min="1" max="10" value="15">
> ```
> ```css
> input:out-of-range { border-color: #f38ba8; }   /* el 15 está fuera de [1,10] */
> ```
> Aplican a `type="number"`, `range`, `date`, `month`, `week`, `time`, `datetime-local`.

## in-range vs. invalid

> [!info] Fuera de rango es un tipo de inválido
> Un valor `:out-of-range` también es `:invalid` (fuera de rango es una forma de no cumplir las reglas). La diferencia: `:out-of-range` es **específico** del problema de límites, lo que permite un mensaje más preciso ("debe estar entre 1 y 10") que un genérico `:invalid`:
> ```css
> input:out-of-range { border-color: #f38ba8; }
> .campo:has(input:out-of-range) .ayuda::after {
>   content: " (fuera del rango permitido)";
> }
> ```

## Feedback específico de límites

> [!tip] Mensaje preciso sobre el rango
> El valor de `:out-of-range` es poder dar un mensaje **específico** sobre el límite, más útil que "valor inválido":
> ```css
> input[type="number"]:out-of-range {
>   border-color: #f38ba8;
> }
> ```
> Combinado con [[03 has() | `:has()`]] y `::after`, se puede mostrar el rango permitido junto al campo automáticamente.

## El problema del prematuro también aplica

Como [[05 valid e invalid | `:invalid`]], `:out-of-range` se activa en cuanto el valor está fuera, lo que puede ser prematuro. Aunque no hay un `:user-out-of-range`, se puede combinar con `:not(:focus)` o gestionar con la lógica de validación general para no marcar antes de tiempo.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:out-of-range` para feedback específico de límites (más preciso que `:invalid`).
> - Asegúrate de que los campos tengan `min`/`max` (sin ellos, no aplican).
> - Da un mensaje que indique el **rango permitido**, no solo "inválido".
> - Combina con `:has()` para mostrar la ayuda en el grupo.

## Errores comunes

> [!warning] Trampas
> - **Esperar que funcione sin `min`/`max`**: necesita un rango definido.
> - **Mensaje genérico**: desaprovecha la especificidad de "fuera de rango".
> - **Marcado prematuro** antes de que el usuario termine.

## Notas relacionadas

- [[05 valid e invalid]] — la validación general (out-of-range es un caso).
- [[07 user-valid y user-invalid]] — validación tras interactuar.
- [[03 Validación HTML5/index]] — los atributos `min`/`max`.
