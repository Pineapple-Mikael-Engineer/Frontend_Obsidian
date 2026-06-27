---
title: Etiquetas y Agrupación — label, fieldset, legend
aliases:
  - form labels
  - etiquetas de formulario
tags:
  - html
  - api/concepto
  - formularios
draft: false
order: 3
---

# Etiquetas y Agrupación

> [!definicion]
> Para que un formulario sea usable y accesible, cada control necesita una **etiqueta** y los grupos de controles relacionados necesitan un **título de grupo**. Lo resuelven tres elementos: el [[01 Etiqueta de Campo (label) | `<label>`]] (etiqueta de un control), el [[02 Agrupación de Campos (fieldset) | `<fieldset>`]] (agrupa controles) y el [[03 Leyenda de Grupo (legend) | `<legend>`]] (titula el grupo).

```html
<fieldset>
  <legend>Datos de contacto</legend>
  <label for="email">Correo</label>
  <input type="email" id="email" name="email" />
</fieldset>
```

## Las tres piezas

| Elemento | Etiqueta de… |
|----------|--------------|
| `<label>` | Un control individual (un input, un select) |
| `<fieldset>` | El contenedor de un grupo de controles |
| `<legend>` | El título del `<fieldset>` (etiqueta del grupo) |

## Por qué son el cimiento de la accesibilidad

> [!info] El error nº 1 en formularios
> Los formularios inaccesibles casi siempre fallan aquí: controles sin `<label>` y grupos de radios/checkboxes sin `<fieldset>`/`<legend>`. Sin etiqueta, un lector de pantalla no sabe qué pide el campo; sin `<legend>`, no sabe a qué pregunta responden las opciones de un grupo. Estos tres elementos son la base, más que cualquier atributo ARIA.

## Mapa de la sección

- [[01 Etiqueta de Campo (label)]] — asociar un texto a un control y ampliar su área clicable.
- [[02 Agrupación de Campos (fieldset)]] — agrupar controles relacionados.
- [[03 Leyenda de Grupo (legend)]] — el título del grupo.

## Notas relacionadas

- [[02 Campos de Entrada (input)/index]] — los controles que se etiquetan.
- [[03 Tipos de input/10 input radio]] — el caso que más necesita `<fieldset>`/`<legend>`.
