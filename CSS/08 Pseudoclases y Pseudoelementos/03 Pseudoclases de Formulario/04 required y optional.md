---
title: required y optional — Campos obligatorios u opcionales
aliases:
  - ":required"
  - ":optional"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":required"
grupo: pseudoclase
draft: false
---

# :required y :optional

> [!definicion]
> `:required` selecciona los campos de formulario marcados como **obligatorios** (con el atributo `required`); `:optional`, los que no lo son. Permiten indicar visualmente qué campos hay que rellenar, sin marcarlos a mano.

```css
input:required { border-left: 3px solid #cba6f7; }
```

## Indicar los campos obligatorios

> [!tip] Marcar visualmente lo que hay que rellenar
> El uso principal: señalar qué campos son obligatorios para que el usuario sepa qué no puede omitir:
> ```css
> input:required {
>   border-left: 3px solid #cba6f7;
> }
> /* Un asterisco automático en el label del campo requerido */
> .campo:has(input:required) label::after {
>   content: " *";
>   color: #f38ba8;
> }
> ```
> Con [[03 has() | `:has()`]], el asterisco se añade solo a los labels de campos `required`, sin tocar el HTML.

## No solo con color

> [!warning] El color no basta para indicar obligatoriedad
> Indicar los campos requeridos **solo con color** (un borde rojo) falla la accesibilidad: quien no distingue colores no lo percibe. Acompaña siempre con otra señal: el **asterisco** (con su significado explicado), la palabra "(obligatorio)", o el atributo `aria-required`. El color es un refuerzo, no la única vía:
> ```css
> .campo:has(input:required) label::after { content: " (obligatorio)"; font-size: 0.85em; }
> ```

## El detalle del asterisco

> [!info] El asterisco necesita explicación
> Si usas el clásico `*` para marcar campos obligatorios, incluye en algún sitio del formulario una leyenda que lo explique ("Los campos marcados con * son obligatorios"), porque el asterisco por sí solo no es universalmente comprensible y los lectores de pantalla pueden leerlo de forma confusa. Mejor aún: usa `aria-required="true"` en el HTML para la asistencia, y el `*` como refuerzo visual.

## required dispara la validación

`:required` se relaciona con [[05 valid e invalid | `:valid`/`:invalid`]]: un campo `required` vacío es `:invalid`. Por eso conviene combinarlos con cuidado para no marcar en rojo campos requeridos vacíos antes de que el usuario interactúe (ver [[07 user-valid y user-invalid]]).

## Buenas prácticas

> [!tip] Recomendaciones
> - Marca los campos `:required` visualmente (borde, asterisco) para guiar al usuario.
> - **No** uses solo el color: añade un asterisco con leyenda o texto "(obligatorio)".
> - Con `:has()`, añade el indicador al label sin tocar el HTML.
> - Usa `aria-required` para la asistencia, no solo el estilo visual.

## Errores comunes

> [!warning] Trampas
> - **Indicar obligatoriedad solo con color**: inaccesible.
> - **Asterisco sin leyenda** que lo explique.
> - **Marcar en rojo** los requeridos vacíos antes de interactuar (usa `:user-invalid`).

## Notas relacionadas

- [[05 valid e invalid]] — la validación que `required` dispara.
- [[07 user-valid y user-invalid]] — validar tras interactuar.
- [[03 has()]] — añadir el indicador al label.
