---
title: <optgroup> — Agrupar opciones de un select
aliases:
  - optgroup
tags:
  - html
  - api/elemento
  - formularios
elemento: optgroup
categoria: interactivo
rol_implicito: group
vacio: false
draft: false
order: 3
---

# Agrupación de Opciones (optgroup)

> [!definicion]
> `<optgroup>` agrupa [[02 Opciones (option) | `<option>`]] relacionadas bajo un **título** dentro de un [[01 Selección (select) | `<select>`]], facilitando la navegación cuando hay muchas opciones. El título se define con el atributo `label`.

```html
<select name="ciudad">
  <optgroup label="España">
    <option value="mad">Madrid</option>
    <option value="bcn">Barcelona</option>
  </optgroup>
  <optgroup label="México">
    <option value="cdmx">Ciudad de México</option>
    <option value="gdl">Guadalajara</option>
  </optgroup>
</select>
```

## El atributo label es obligatorio

El `label` del `<optgroup>` es el texto del encabezado del grupo, que se muestra **en negrita y no es seleccionable**. A diferencia de las opciones, el grupo solo organiza: no es una opción elegible en sí.

| Atributo | Efecto |
|----------|--------|
| `label` | Título del grupo (obligatorio, visible) |
| `disabled` | Deshabilita **todas** las opciones del grupo a la vez |

## disabled en cascada

Poner `disabled` en el `<optgroup>` deshabilita todas sus opciones de una vez, útil para "categorías no disponibles":

```html
<optgroup label="Próximamente" disabled>
  <option>Talla XXL</option>
</optgroup>
```

## Limitaciones

| Limitación | Detalle |
|------------|---------|
| No se anidan | Un `<optgroup>` no puede contener otro `<optgroup>` |
| Solo un nivel | La agrupación es de un único nivel de profundidad |
| Estilado limitado | Como el `<select>`, su apariencia es poco personalizable |

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<optgroup>` cuando un `<select>` tenga muchas opciones agrupables por categoría.
> - Pon siempre un `label` descriptivo.
> - Usa `disabled` en el grupo para desactivar categorías completas.
> - Si necesitas más de un nivel de jerarquía, replantea el control (selects dependientes con JS).

## Errores comunes

> [!warning] Trampas
> - **Intentar anidar `<optgroup>`**: no está permitido; solo un nivel.
> - **Olvidar `label`**: el grupo queda sin título.
> - **Esperar que el grupo sea seleccionable**: solo organiza, no es una opción.

## Notas relacionadas

- [[01 Selección (select)]] — el contenedor del desplegable.
- [[02 Opciones (option)]] — las opciones que se agrupan.
