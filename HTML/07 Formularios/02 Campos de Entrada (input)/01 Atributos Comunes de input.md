---
title: Atributos comunes de input — name, value, placeholder, required…
aliases:
  - input attributes
  - atributos input
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

# Atributos Comunes de input

> [!definicion]
> La mayoría de los [[03 Tipos de input/index | tipos de `<input>`]] comparten un conjunto de atributos que controlan su nombre, valor inicial, estado y comportamiento. Conocerlos una vez evita repetirlos en cada tipo.

```html
<input type="text" name="usuario" id="usuario"
       value="" placeholder="ej. ana_lopez"
       required maxlength="20" autocomplete="username" />
```

## Identidad y valor

| Atributo | Para qué |
|----------|----------|
| `name` | Clave con la que el valor se envía al servidor (**sin él no se envía**) |
| `id` | Identificador para asociar el `<label for>` y para CSS/JS |
| `value` | Valor inicial (y valor actual del control) |
| `placeholder` | Texto de ejemplo que desaparece al escribir (no es etiqueta) |

`name` e `id` cumplen funciones distintas: `name` es para el envío de datos; `id` es para enlazar el `<label>`. Lo normal es que un campo tenga ambos.

## Estado del control

| Atributo | Efecto |
|----------|--------|
| `required` | El campo debe rellenarse para enviar el formulario |
| `disabled` | Deshabilitado: no editable y **no se envía** |
| `readonly` | Solo lectura: no editable pero **sí se envía** |
| `autofocus` | Recibe el foco al cargar la página (usar con moderación) |

> [!info] disabled vs. readonly: la diferencia clave
> Ambos impiden editar, pero se comportan distinto al enviar y para la asistencia:
> | | `disabled` | `readonly` |
> |--|-----------|-----------|
> | ¿Se envía su valor? | **No** | **Sí** |
> | ¿Recibe foco? | No | Sí |
> | ¿Lo anuncia el lector? | A menudo lo salta | Sí, como "solo lectura" |
>
> Usa `readonly` para un valor que el usuario debe ver y enviar pero no cambiar (un total calculado); `disabled` para un control temporalmente inactivo cuyo valor no interesa.

## Restricciones de validación

Comunes a varios tipos (el detalle, en [[09 Validación de Formularios/03 Restricciones (required, min, max, step, maxlength) | Restricciones]]):

| Atributo | Aplica a | Efecto |
|----------|----------|--------|
| `required` | casi todos | Obligatorio |
| `minlength` / `maxlength` | texto | Longitud mínima/máxima de caracteres |
| `min` / `max` | número, fecha, range | Valor mínimo/máximo |
| `step` | número, fecha, range | Incremento permitido |
| `pattern` | texto | Expresión regular que debe cumplir |

## Autocompletado

`autocomplete` ayuda al navegador (y a los gestores de contraseñas) a rellenar campos. Usar los **valores estándar** mejora mucho la experiencia:

```html
<input type="email" name="email" autocomplete="email" />
<input type="password" autocomplete="current-password" />
<input type="text" name="nombre" autocomplete="name" />
```

Valores comunes: `name`, `email`, `tel`, `street-address`, `postal-code`, `cc-number`, `current-password`, `new-password`, `one-time-code`.

## placeholder no es label

> [!warning] El placeholder no sustituye a la etiqueta
> El `placeholder` desaparece al escribir, suele tener contraste insuficiente y no lo anuncian de forma fiable todos los lectores de pantalla. Un campo cuya única "etiqueta" es el placeholder deja al usuario sin saber qué escribió una vez empieza. Usa siempre un [[07 Etiquetas y Agrupación/01 Etiqueta de Campo (label) | `<label>`]] visible, y el placeholder como **ejemplo** opcional.

## Buenas prácticas

> [!tip] Recomendaciones
> - `name` en todo campo que deba enviarse; `id` para el `<label>`.
> - `readonly` si el valor debe enviarse sin editarse; `disabled` si no debe enviarse.
> - `autocomplete` con valores estándar para acelerar el rellenado.
> - `autofocus` solo en formularios de un único propósito (un buscador), no en formularios largos.

## Errores comunes

> [!warning] Trampas
> - **Olvidar `name`**: el campo no envía nada.
> - **Confundir `disabled` con `readonly`** al esperar el valor en el servidor.
> - **placeholder como etiqueta**: problema de accesibilidad.
> - **`autocomplete="off"` en contraseñas**: estorba a los gestores de contraseñas y empeora la seguridad práctica.

## Notas relacionadas

- [[02 Campos de Entrada (input)/index]] — visión general del `<input>`.
- [[02 Validación Específica por Tipo]] — cómo valida cada tipo concreto.
- [[07 Etiquetas y Agrupación/01 Etiqueta de Campo (label)]] — la etiqueta obligatoria.
