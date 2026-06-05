---
title: Listas de Selección — select, option, optgroup
aliases:
  - select dropdown
  - desplegables
tags:
  - html
  - api/concepto
  - formularios
draft: false
---

# Listas de Selección (select)

> [!definicion]
> Una lista de selección (desplegable) deja elegir entre **muchas opciones** ocupando poco espacio. La construyen tres elementos: el [[01 Selección (select) | `<select>`]] (el contenedor desplegable), las [[02 Opciones (option) | `<option>`]] (cada alternativa) y, opcionalmente, los [[03 Agrupación de Opciones (optgroup) | `<optgroup>`]] (grupos de opciones).

```html
<label for="pais">País</label>
<select id="pais" name="pais">
  <option value="">— Elige —</option>
  <option value="es">España</option>
  <option value="mx">México</option>
  <option value="ar">Argentina</option>
</select>
```

## Las tres piezas

| Elemento | Función | Nota |
|----------|---------|------|
| `<select>` | El control desplegable | [[01 Selección (select)]] |
| `<option>` | Cada opción elegible | [[02 Opciones (option)]] |
| `<optgroup>` | Agrupar opciones bajo un título | [[03 Agrupación de Opciones (optgroup)]] |

## Cuándo un select y cuándo radios

| Usa `<select>` | Usa radios / checkboxes |
|----------------|-------------------------|
| Muchas opciones (países, años) | Pocas opciones (2–5) |
| Espacio limitado | Quieres todas visibles a la vez |
| Una sola elección (o `multiple`) | Comparación rápida entre opciones |

Para pocas opciones, los [[03 Tipos de input/10 input radio | radios]] suelen ser más usables porque se ven todas sin desplegar.

## El valor que se envía

El `<select>` envía el `value` de la `<option>` seleccionada bajo el `name` del select. Si la opción no tiene `value`, se envía su texto. Una primera opción "placeholder" con `value=""` y `disabled`/`selected` obliga a elegir conscientemente.

## Notas relacionadas

- [[01 Selección (select)]] — el contenedor y sus atributos (`multiple`, `size`).
- [[03 Tipos de input/10 input radio]] — alternativa para pocas opciones.
- [[05 Lista de Datos (datalist)]] — sugerencias editables, distinto de select.
