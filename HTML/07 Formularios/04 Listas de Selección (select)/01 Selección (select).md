---
title: <select> — Lista desplegable
aliases:
  - select element
tags:
  - html
  - api/elemento
  - formularios
elemento: select
categoria: interactivo
rol_implicito: combobox
vacio: false
draft: false
---

# Selección (select)

> [!definicion]
> `<select>` es el control **desplegable** que contiene las [[02 Opciones (option) | `<option>`]]. Por defecto muestra una opción y despliega el resto al pulsarlo. Con atributos puede convertirse en una lista de selección múltiple.

```html
<label for="talla">Talla</label>
<select id="talla" name="talla">
  <option value="s">S</option>
  <option value="m" selected>M</option>
  <option value="l">L</option>
</select>
```

## Atributos

| Atributo | Efecto |
|----------|--------|
| `name` | Clave con la que se envía el valor seleccionado |
| `multiple` | Permite seleccionar **varias** opciones |
| `size` | Número de opciones visibles a la vez (lo convierte en lista, no desplegable) |
| `required` | Obliga a elegir una opción con `value` no vacío |
| `disabled` | Deshabilita el control |

## Selección múltiple

Con `multiple`, el `<select>` se muestra como una lista y permite elegir varias opciones (con Ctrl/Cmd o arrastrando):

```html
<select name="generos" multiple size="4">
  <option value="rock">Rock</option>
  <option value="pop">Pop</option>
  <option value="jazz">Jazz</option>
</select>
```

> [!info] multiple cambia el envío
> Con `multiple`, se envían **varios pares** con el mismo `name` (`generos=rock&generos=jazz`), igual que un grupo de checkboxes. De hecho, para selección múltiple, un grupo de [[03 Tipos de input/09 input checkbox | checkboxes]] suele ser más usable que un `select multiple`, que es poco intuitivo (requiere Ctrl/Cmd).

## La opción placeholder

Para forzar una elección consciente, se pone una primera opción vacía, deshabilitada y seleccionada:

```html
<select required>
  <option value="" disabled selected>— Elige una opción —</option>
  <option value="es">España</option>
</select>
```

Con `required`, el formulario no se envía si queda en la opción de `value=""`.

## Estilado limitado

> [!warning] El select nativo es difícil de estilar
> La apariencia del desplegable nativo (sobre todo la lista de opciones al abrirse) **apenas se puede personalizar** con CSS, y varía entre navegadores y sistemas. Para un desplegable totalmente a medida se construye uno con JavaScript y ARIA (`role="listbox"`), pero eso sacrifica la accesibilidad y robustez del nativo. Cuando basta el estándar, el `<select>` nativo es más accesible y ligero.

## Buenas prácticas

> [!tip] Recomendaciones
> - Asocia siempre un `<label>`.
> - Usa una opción placeholder + `required` para forzar elección.
> - Para selección múltiple, valora checkboxes en lugar de `multiple`.
> - Agrupa opciones largas con [[03 Agrupación de Opciones (optgroup) | `<optgroup>`]].

## Errores comunes

> [!warning] Trampas
> - **`select multiple` para multiselección** cuando unos checkboxes serían más claros.
> - **Sin opción placeholder** y sin valor por defecto intencionado.
> - **Intentar estilar la lista desplegada** con CSS esperando control total.

## Notas relacionadas

- [[02 Opciones (option)]] — las opciones que contiene.
- [[03 Agrupación de Opciones (optgroup)]] — agrupar opciones.
- [[03 Tipos de input/09 input checkbox]] — alternativa para selección múltiple.
