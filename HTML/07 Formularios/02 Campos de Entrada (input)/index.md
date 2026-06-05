---
title: <input> — Campo de entrada (visión general)
aliases:
  - input element
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: textbox
vacio: true
draft: false
---

# Campos de Entrada (input)

> [!definicion]
> `<input>` es el control de formulario más versátil: un único elemento void que, según su atributo `type`, se convierte en un campo de texto, una casilla, un selector de fecha, un control de color o un botón. El `type` es lo que determina su apariencia, su teclado en móvil y su validación.

```html
<input type="text" name="nombre" placeholder="Tu nombre" />
<input type="email" name="correo" required />
<input type="checkbox" name="acepto" id="acepto" />
```

## Un elemento, muchos comportamientos

El mismo `<input>` cambia por completo según `type`. Los 22 tipos se agrupan así:

| Grupo | Tipos | Nota |
|-------|-------|------|
| Texto | `text`, `password`, `email`, `search`, `url`, `tel` | varias |
| Numérico | `number`, `range` | varias |
| Fecha/hora | `date`, `datetime-local`, `month`, `week`, `time` | varias |
| Selección | `checkbox`, `radio` | varias |
| Especiales | `file`, `color`, `hidden`, `image` | varias |
| Botones | `submit`, `reset`, `button` | varias |

El catálogo completo está en [[03 Tipos de input/index | Tipos de input]].

## Las tres notas transversales

Esta carpeta separa lo que es común a casi todos los tipos de lo que es propio de cada uno:

- [[01 Atributos Comunes de input]] — `name`, `value`, `placeholder`, `required`, `disabled`, `readonly`… los atributos que comparten casi todos los tipos.
- [[02 Validación Específica por Tipo]] — cómo cada `type` valida su valor (un `email` exige `@`, un `number` rechaza letras).
- [[03 Tipos de input/index]] — una nota por cada uno de los 22 tipos.

## input es void: sin etiqueta de cierre

`<input>` no tiene contenido ni etiqueta de cierre: todo se configura con atributos. Por eso, a diferencia de [[06 Botones (button) | `<button>`]], no puede contener HTML (un `<input type="submit">` muestra texto vía el atributo `value`, no como contenido).

## Siempre con su label

> [!warning] input sin label es inaccesible
> Un `<input>` **siempre** debe tener un [[07 Etiquetas y Agrupación/01 Etiqueta de Campo (label) | `<label>`]] asociado. El `placeholder` **no sustituye** a la etiqueta: desaparece al escribir, tiene bajo contraste y muchos lectores de pantalla no lo anuncian de forma fiable.
> ```html
> <label for="nombre">Nombre</label>
> <input type="text" id="nombre" name="nombre" />
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Elige el `type` más específico: mejora el teclado móvil, la validación y el autocompletado.
> - Da `name` (para el envío) e `id` (para el `<label>`) a cada campo.
> - Usa `placeholder` como ejemplo, no como etiqueta.
> - Aprovecha `autocomplete` con valores estándar (`email`, `name`, `current-password`).

## Notas relacionadas

- [[01 Atributos Comunes de input]] — los atributos compartidos.
- [[03 Tipos de input/index]] — el catálogo de los 22 tipos.
- [[07 Etiquetas y Agrupación/01 Etiqueta de Campo (label)]] — la etiqueta obligatoria.
