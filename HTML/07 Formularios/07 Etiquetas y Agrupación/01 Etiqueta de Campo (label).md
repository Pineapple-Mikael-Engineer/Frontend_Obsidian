---
title: <label> — Etiqueta de un control de formulario
aliases:
  - label element
tags:
  - html
  - api/elemento
  - formularios
elemento: label
categoria: interactivo
rol_implicito: ninguno
vacio: false
draft: false
order: 1
---

# Etiqueta de Campo (label)

> [!definicion]
> `<label>` asocia un **texto descriptivo** a un control de formulario. Es imprescindible: identifica el campo para los lectores de pantalla y **amplía el área clicable** (pulsar la etiqueta enfoca o activa el control). Todo control debe tener su `<label>`.

```html
<label for="correo">Correo electrónico</label>
<input type="email" id="correo" name="correo" />
```

## Dos formas de asociar

Hay dos maneras de vincular un `<label>` con su control, ambas válidas:

### Asociación explícita (for + id)

El `for` del label apunta al `id` del control. Funciona aunque no estén juntos en el marcado:

```html
<label for="nombre">Nombre</label>
<input type="text" id="nombre" name="nombre" />
```

### Asociación implícita (envolver)

El control va **dentro** del `<label>`; no hace falta `for`/`id`:

```html
<label>
  Nombre
  <input type="text" name="nombre" />
</label>
```

| Forma | Cuándo conviene |
|-------|-----------------|
| Explícita (`for`/`id`) | Lo más flexible; control y etiqueta pueden estar separados |
| Implícita (envolver) | Cómoda con checkboxes/radios; no requiere `id` |

## Por qué amplía el área clicable

> [!info] Un beneficio tangible para todos
> Al asociar el `<label>`, hacer clic en el texto **activa el control**. Esto es especialmente valioso en checkboxes y radios, donde el cuadradito/círculo es diminuto: con `<label>`, el usuario puede pulsar el texto entero. Beneficia a todos, no solo a quien usa lector de pantalla, y reduce los errores de clic en móvil.

## El placeholder no es un label

> [!warning] Nunca sustituyas el label por el placeholder
> Es tentador ahorrar espacio usando solo el `placeholder` ("Correo" dentro del campo) y quitar la etiqueta. Es un error de accesibilidad clásico:
> - El placeholder **desaparece** al escribir: el usuario olvida qué campo era.
> - Suele tener **contraste insuficiente**.
> - No todos los lectores de pantalla lo anuncian de forma fiable.
>
> El `<label>` siempre visible es obligatorio; el placeholder es un ejemplo opcional **adicional**.

## Una etiqueta por control

Cada `<label>` etiqueta **un** control. Para etiquetar un **grupo** (varios radios), no se usa `<label>` sino [[03 Leyenda de Grupo (legend) | `<legend>`]] dentro de un [[02 Agrupación de Campos (fieldset) | `<fieldset>`]].

## Buenas prácticas

> [!tip] Recomendaciones
> - **Todo** control con su `<label>` (explícito con `for`/`id`, o envolvente).
> - Etiqueta siempre visible; el placeholder es solo un ejemplo.
> - Para un grupo de opciones, usa `<fieldset>` + `<legend>`, no un `<label>` suelto.
> - Texto de etiqueta claro y conciso ("Correo", no "Introduzca aquí su dirección de correo").

## Errores comunes

> [!warning] Trampas
> - **Control sin `<label>`**: inaccesible y con área clicable mínima.
> - **Placeholder como etiqueta**: desaparece y falla con lectores de pantalla.
> - **`for` que no coincide con ningún `id`**: la asociación se rompe silenciosamente.

## Notas relacionadas

- [[02 Agrupación de Campos (fieldset)]] — para etiquetar grupos, no controles sueltos.
- [[03 Leyenda de Grupo (legend)]] — el "label" de un grupo.
- [[01 Atributos Comunes de input]] — el `id` que el `for` referencia.
