---
title: input type="reset" — Botón de restablecer
aliases:
  - input reset
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: button
vacio: true
draft: false
order: 21
---

# input reset

> [!definicion]
> `<input type="reset">` es un botón que **restablece** todos los campos del formulario a sus valores iniciales (los que tenían al cargar la página, no vacíos). Su texto se define con `value`.

```html
<input type="reset" value="Limpiar formulario" />
```

## Qué hace exactamente

> [!info] Vuelve al valor inicial, no a vacío
> Reset no "vacía" el formulario: lo devuelve al estado **inicial**. Un campo con `value="Ana"` al cargar vuelve a "Ana", no a vacío; una casilla con `checked` vuelve a marcada. Restaura los valores por defecto declarados en el HTML.

## Por qué casi nunca debe usarse

> [!warning] Reset suele hacer más daño que bien
> El botón reset es **peligroso para la usabilidad**: está justo al lado del de envío y un clic accidental **borra todo el trabajo** del usuario, sin confirmación ni deshacer. Es una de las recomendaciones clásicas de UX: **no incluir un botón reset** en la mayoría de formularios. El usuario rara vez quiere "empezar de cero"; sí quiere corregir un campo concreto, cosa que reset no facilita.

## Cuándo (si acaso) tiene sentido

Casos muy puntuales: un formulario de configuración con "restaurar valores por defecto" claramente separado del envío, o herramientas donde "limpiar y reempezar" es una acción frecuente y esperada. Incluso entonces, conviene separarlo visualmente y, mejor, pedir confirmación.

## Buenas prácticas

> [!tip] Recomendaciones
> - **Por defecto, no pongas un botón reset.**
> - Si lo incluyes, sepáralo claramente del botón de envío y considera una confirmación.
> - Para "limpiar un campo", ofrece controles individuales (la "x" de [[07 input search | search]]), no un reset global.

## Errores comunes

> [!warning] Trampas
> - **Reset junto a submit**: invita al clic accidental que borra todo.
> - **Esperar que vacíe**: vuelve a los valores iniciales, no a vacío.
> - **Incluirlo "por costumbre"**: rara vez aporta y a menudo perjudica.

## Notas relacionadas

- [[20 input submit]] — el botón de envío, del que conviene separarlo.
- [[06 Botones (button)]] — `<button type="reset">`, equivalente más flexible.
