---
title: input type="date" — Selector de fecha
aliases:
  - input date
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: ninguno
vacio: true
draft: false
---

# input date

> [!definicion]
> `<input type="date">` permite elegir una **fecha** (año, mes y día) mediante el selector de calendario nativo del navegador. El valor se almacena siempre en formato ISO `AAAA-MM-DD`, independientemente de cómo se muestre al usuario según su configuración regional.

```html
<label for="nacimiento">Fecha de nacimiento</label>
<input type="date" id="nacimiento" name="nacimiento" min="1900-01-01" max="2026-12-31" />
```

## Formato de almacenamiento vs. visualización

> [!info] El valor siempre es ISO
> Hay que distinguir dos cosas:
> - **Valor** (`value` y lo que se envía): siempre `AAAA-MM-DD` (`2026-06-04`), un formato universal.
> - **Visualización**: el navegador la adapta a la región del usuario (`04/06/2026` en España, `6/4/2026` en EE. UU.).
>
> El desarrollador trabaja con el formato ISO; el usuario ve su formato local. Esto evita la ambigüedad clásica de "¿es día/mes o mes/día?".

## min, max y step

| Atributo | Efecto |
|----------|--------|
| `min` | Fecha más temprana seleccionable (ISO) |
| `max` | Fecha más tardía seleccionable (ISO) |
| `step` | Salto en días (`step="7"` = semanal); por defecto 1 día |

```html
<!-- Solo fechas futuras -->
<input type="date" min="2026-06-05" />
```

## Limitaciones de estilo y control

> [!warning] El widget nativo es poco personalizable
> El selector de calendario nativo **no se puede estilar** apenas con CSS y varía mucho entre navegadores y sistemas. Si el diseño exige un calendario a medida (rangos, días deshabilitados, multi-idioma uniforme), se recurre a una librería de date picker en JavaScript. El tipo nativo es ideal cuando basta la funcionalidad estándar y se prioriza accesibilidad y peso.

## Buenas prácticas

> [!tip] Recomendaciones
> - Acota con `min`/`max` cuando el rango es conocido (fechas futuras, mayoría de edad).
> - Recuerda que envías/recibes ISO `AAAA-MM-DD`; convierte para mostrar si lo necesitas en otro formato fuera del control.
> - Si necesitas control visual total, usa una librería; si no, el nativo es más accesible y ligero.

## Errores comunes

> [!warning] Trampas
> - **Asumir el formato visible** (`DD/MM/AAAA`) al procesar: el valor siempre es ISO.
> - **Esperar estilar el calendario** con CSS: el widget nativo es casi inmutable.
> - **No acotar** rangos imposibles (fecha de nacimiento en el futuro).

## Notas relacionadas

- [[13 input datetime-local]] — fecha **y** hora juntas.
- [[14 input month]] · [[15 input week]] · [[16 input time]] — variantes de fecha/hora.
- [[23 Tiempo (time)]] — el elemento `<time>` para mostrar fechas (no confundir).
