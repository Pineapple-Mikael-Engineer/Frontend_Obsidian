---
title: disabled y enabled — Campos deshabilitados o habilitados
aliases:
  - ":disabled"
  - ":enabled"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":disabled"
grupo: pseudoclase
draft: false
---

# :disabled y :enabled

> [!definicion]
> `:disabled` selecciona los campos de formulario **deshabilitados** (con el atributo `disabled`), que no se pueden editar ni enviar. `:enabled` selecciona los habilitados (el estado por defecto). Permiten dar un aspecto "apagado" a los controles inactivos.

```css
input:disabled { opacity: 0.5; cursor: not-allowed; background: #f0f0f0; }
```

## El aspecto de "deshabilitado"

> [!tip] Comunicar que un control está inactivo
> Un campo deshabilitado debe **verse** inactivo para que el usuario no intente usarlo. Las convenciones: opacidad reducida, cursor de "prohibido", fondo gris:
> ```css
> button:disabled,
> input:disabled {
>   opacity: 0.5;
>   cursor: not-allowed;
>   background: #f5f5f5;
> }
> ```

## El contraste de accesibilidad

> [!warning] No reduzcas tanto el contraste que sea ilegible
> Aunque un campo deshabilitado se ve "apagado", no debe quedar **ilegible**. Reducir mucho la opacidad puede bajar el contraste por debajo del mínimo, dificultando leer qué dice el campo (importante para entender por qué no se puede usar). Busca un equilibrio: claramente "apagado" pero aún legible. Las pautas de contraste son algo más laxas para controles deshabilitados, pero la legibilidad importa.

## disabled vs. readonly

> [!info] Deshabilitado no se envía; solo lectura sí
> Diferencia clave con [[03 read-only y read-write | `:read-only`]]:
> - **`disabled`**: el campo está inactivo, **no se puede enfocar ni editar**, y **no se envía** con el formulario.
> - **`readonly`**: el campo **no se edita** pero **sí se enfoca** y **sí se envía**.
>
> Usa `disabled` para campos que no aplican; `readonly` para mostrar un valor fijo que debe enviarse.

## Estilar el contenedor con :has()

> [!tip] Atenuar todo el grupo de un campo deshabilitado
> Con [[03 has() | `:has()`]], se puede atenuar el **grupo entero** (label incluido) cuando su campo está deshabilitado:
> ```css
> .campo:has(input:disabled) { opacity: 0.5; }   /* el label y el campo se apagan juntos */
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Da a los controles `:disabled` un aspecto apagado (opacidad, cursor `not-allowed`).
> - Mantén el texto **legible** pese a la atenuación (no bajes tanto el contraste).
> - Usa `disabled` para campos inactivos que no se envían; `readonly` si deben enviarse.
> - Con `:has()`, atenúa el grupo entero cuando el campo está deshabilitado.

## Errores comunes

> [!warning] Trampas
> - **Texto ilegible** por exceso de atenuación.
> - **Confundir `disabled` con `readonly`**: uno no se envía, el otro sí.
> - **Sin `cursor: not-allowed`**: el usuario no percibe que está inactivo.

## Notas relacionadas

- [[03 read-only y read-write]] — la alternativa que sí se envía.
- [[03 has()]] — atenuar el grupo del campo.
- [[06 Personalización de Controles (accent-color, appearance)]] — estilizar campos.
