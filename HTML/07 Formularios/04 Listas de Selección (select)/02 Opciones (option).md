---
title: <option> — Opción de una lista de selección
aliases:
  - option element
tags:
  - html
  - api/elemento
  - formularios
elemento: option
categoria: interactivo
rol_implicito: option
vacio: false
draft: false
---

# Opciones (option)

> [!definicion]
> `<option>` representa una **opción** dentro de un [[01 Selección (select) | `<select>`]] (o de un [[05 Lista de Datos (datalist) | `<datalist>`]]). El texto entre las etiquetas es lo que ve el usuario; el atributo `value` es lo que se envía.

```html
<option value="es">España</option>
```

## value vs. texto visible

| | Atributo `value` | Texto entre etiquetas |
|--|------------------|------------------------|
| Qué es | Lo que se **envía** al servidor | Lo que **ve** el usuario |
| Si falta `value` | — | Se envía el texto visible |

```html
<!-- Se ve "España", se envía "es" -->
<option value="es">España</option>

<!-- Sin value: se ve y se envía "España" -->
<option>España</option>
```

Separar `value` (código estable, `es`) del texto (legible, `España`) permite cambiar la etiqueta sin tocar el dato que procesa el servidor.

## Atributos

| Atributo | Efecto |
|----------|--------|
| `value` | Valor enviado si se elige esta opción |
| `selected` | Opción seleccionada por defecto |
| `disabled` | Opción no seleccionable (atenuada) |
| `label` | Etiqueta alternativa (poco usada; el texto suele bastar) |

## La opción placeholder

Una primera opción vacía y deshabilitada sirve de marcador y, con `required` en el select, fuerza una elección:

```html
<option value="" disabled selected>— Elige —</option>
```

`disabled` impide volver a seleccionarla; `selected` la muestra de inicio; `value=""` hace que `required` la considere "sin elegir".

## Buenas prácticas

> [!tip] Recomendaciones
> - Da `value` estable (un código) y texto legible: desacopla dato de etiqueta.
> - Usa una opción placeholder deshabilitada para forzar elección.
> - Marca con `selected` un valor por defecto razonable cuando exista.
> - Agrupa opciones relacionadas con [[03 Agrupación de Opciones (optgroup) | `<optgroup>`]].

## Errores comunes

> [!warning] Trampas
> - **Sin `value`** cuando el texto visible no es un buen dato (acentos, espacios): se envía el texto tal cual.
> - **Placeholder sin `disabled`**: el usuario puede "elegir" la opción vacía.
> - **HTML dentro de `<option>`**: solo admite texto plano, no etiquetas.

## Notas relacionadas

- [[01 Selección (select)]] — el contenedor de las opciones.
- [[03 Agrupación de Opciones (optgroup)]] — agrupar opciones bajo un título.
- [[05 Lista de Datos (datalist)]] — las `<option>` también se usan aquí, como sugerencias.
