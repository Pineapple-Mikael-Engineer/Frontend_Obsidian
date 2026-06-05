---
title: Restricciones — required, min, max, step, maxlength
aliases:
  - constraint attributes
  - restricciones
tags:
  - html
  - api/atributo
  - formularios
elemento: input
categoria: interactivo
rol_implicito: ninguno
vacio: true
draft: false
---

# Restricciones (required, min, max, step, maxlength)

> [!definicion]
> Un conjunto de atributos impone **límites** a lo que un control acepta, y el navegador los valida automáticamente. Son la base de la [[01 Validación Nativa HTML5 | validación nativa]] junto con `type` y [[02 Atributo pattern | `pattern`]].

```html
<input type="text" required minlength="3" maxlength="20" />
<input type="number" min="0" max="100" step="5" />
```

## Catálogo de restricciones

| Atributo | Aplica a | Efecto |
|----------|----------|--------|
| `required` | Casi todos | El campo no puede quedar vacío |
| `minlength` | Texto | Mínimo de caracteres |
| `maxlength` | Texto | Máximo de caracteres (también impide teclear más) |
| `min` | número, fecha, range | Valor mínimo |
| `max` | número, fecha, range | Valor máximo |
| `step` | número, fecha, range | Incremento permitido |

## required: el más usado

`required` marca el campo como obligatorio. En checkboxes obliga a marcarlo; en un grupo de radios, basta `required` en uno para exigir elegir alguno; en `<select>`, exige una opción con `value` no vacío.

```html
<input type="email" required />
<input type="checkbox" required />   <!-- aceptar términos -->
```

## min/max/step: el detalle de step

`step` no es solo el salto de las flechas: define **qué valores son válidos**. Con `step="5"` y `min="0"`, son válidos 0, 5, 10…, pero 3 es inválido:

```html
<input type="number" min="0" max="100" step="5" />   <!-- 0,5,10… -->
<input type="number" step="0.01" />                   <!-- 2 decimales -->
<input type="number" step="any" />                    <!-- cualquier decimal -->
```

`step` también se aplica a fechas/horas (en días o segundos). Más en [[03 Tipos de input/04 input number | number]] y [[03 Tipos de input/16 input time | time]].

## minlength/maxlength vs. min/max

> [!warning] No confundir longitud con valor
> Un error frecuente: usar `max` para limitar **caracteres**. Son cosas distintas:
> - `maxlength="5"` → máximo 5 **caracteres** (para texto).
> - `max="5"` → valor numérico máximo de **5** (para number/date/range).
>
> Para un campo de código de 5 dígitos, lo correcto es `maxlength="5"` (o `pattern="[0-9]{5}"`), no `max`.

## maxlength impide teclear de más

A diferencia de las demás restricciones (que solo marcan inválido al enviar), `maxlength` **impide físicamente** escribir más caracteres. `minlength`, en cambio, solo invalida al enviar si no se alcanza.

## Buenas prácticas

> [!tip] Recomendaciones
> - Marca como `required` todos los campos obligatorios; indícalo también visualmente (asterisco, texto).
> - Usa `minlength`/`maxlength` para texto y `min`/`max` para valores; no los confundas.
> - Define `step` acorde a la granularidad real (decimales, intervalos).
> - Recuerda: las restricciones son saltables; revalida en servidor.

## Errores comunes

> [!warning] Trampas
> - **`max` para limitar caracteres**: usa `maxlength`.
> - **`required` sin señal visual**: el usuario no sabe qué campos son obligatorios hasta fallar.
> - **`step` olvidado** cuando se necesitan decimales o intervalos concretos.

## Notas relacionadas

- [[01 Atributos Comunes de input]] — donde estos atributos se introducen.
- [[02 Atributo pattern]] — restricción de formato con regex.
- [[04 Pseudoclases de Validación]] — estilar según se cumplan o no las restricciones.
