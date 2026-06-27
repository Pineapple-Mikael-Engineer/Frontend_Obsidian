---
title: input type="radio" — Botón de opción
aliases:
  - input radio
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: radio
vacio: true
draft: false
order: 10
---

# input radio

> [!definicion]
> `<input type="radio">` es un **botón de opción**: permite elegir **una sola** alternativa de un grupo. Marcar una opción desmarca automáticamente las demás del mismo grupo. El grupo se define compartiendo el atributo `name`.

```html
<fieldset>
  <legend>Tamaño</legend>
  <input type="radio" id="s" name="talla" value="S" />
  <label for="s">Pequeña</label>
  <input type="radio" id="m" name="talla" value="M" checked />
  <label for="m">Mediana</label>
  <input type="radio" id="l" name="talla" value="L" />
  <label for="l">Grande</label>
</fieldset>
```

## El name compartido define el grupo

> [!info] Mismo name = exclusión mutua
> El comportamiento "solo una" funciona porque todos los radios del grupo comparten el **mismo `name`**. Si cada radio tiene un `name` distinto, dejan de ser excluyentes y se pueden marcar varios a la vez (un error frecuente). El `value` —que sí es distinto en cada uno— es lo que se envía:
> ```
> talla=M
> ```

## Atributos

| Atributo | Efecto |
|----------|--------|
| `name` | **Define el grupo** (igual en todos los radios excluyentes) |
| `value` | Valor que se envía si ese radio está marcado (distinto en cada uno) |
| `checked` | Opción seleccionada por defecto |
| `required` | Obliga a elegir una opción del grupo |

## Agrupar con fieldset y legend

Un grupo de radios debería ir dentro de un [[07 Etiquetas y Agrupación/02 Agrupación de Campos (fieldset) | `<fieldset>`]] con [[07 Etiquetas y Agrupación/03 Leyenda de Grupo (legend) | `<legend>`]], que actúa como "etiqueta del grupo entero". Así el lector de pantalla anuncia "Tamaño: Mediana, opción 2 de 3".

## radio vs. checkbox vs. select

| Control | Cuándo |
|---------|--------|
| `radio` | Elegir **una** de pocas opciones (2–5), todas visibles |
| `checkbox` | Elegir **varias** (o una opción binaria) |
| `select` | Elegir una de **muchas** opciones (ahorra espacio) |

## Buenas prácticas

> [!tip] Recomendaciones
> - Comparte el mismo `name` en todo el grupo; `value` distinto en cada uno.
> - Agrupa con `<fieldset>` + `<legend>`.
> - Marca una opción por defecto con `checked` cuando tenga sentido un valor inicial.
> - Para más de ~5 opciones, considera un [[01 Selección (select) | `<select>`]].

## Errores comunes

> [!warning] Trampas
> - **`name` distinto en cada radio**: deja de ser excluyente; se marcan varios.
> - **Grupo sin `<fieldset>`/`<legend>`**: el lector no sabe a qué pregunta responden las opciones.
> - **Sin opción por defecto** cuando se requiere elegir y no hay `required`.

## Notas relacionadas

- [[09 input checkbox]] — para selección múltiple.
- [[01 Selección (select)]] — para muchas opciones en poco espacio.
- [[07 Etiquetas y Agrupación/02 Agrupación de Campos (fieldset)]] — agrupar el conjunto de opciones.
