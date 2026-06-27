---
title: input type="button" — Botón genérico
aliases:
  - input button
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: button
vacio: true
draft: false
order: 22
---

# input button

> [!definicion]
> `<input type="button">` es un botón **sin comportamiento por defecto**: no envía ni restablece el formulario. Solo hace algo si se le asocia una acción con JavaScript. Su texto se define con `value`.

```html
<input type="button" value="Calcular" id="calc" />
```

```js
document.getElementById("calc").addEventListener("click", () => {
  // acción personalizada
});
```

## Los tres tipos de botón comparados

| Tipo | Comportamiento por defecto |
|------|----------------------------|
| `type="submit"` | Envía el formulario |
| `type="reset"` | Restablece el formulario |
| `type="button"` | **Nada**: solo lo que le añada JavaScript |

## input button vs. button

Casi siempre es preferible [[06 Botones (button) | `<button type="button">`]] frente a `<input type="button">`, por la misma razón que con submit: `<button>` admite **contenido HTML** (iconos, varios elementos), mientras que `input button` solo muestra el texto de `value`.

```html
<!-- input: solo texto -->
<input type="button" value="Menú" />

<!-- button: texto + icono -->
<button type="button"><svg>…</svg> Menú</button>
```

## Cuándo se usa

`type="button"` es para acciones **dentro de un formulario** que no deben enviarlo: un "calcular subtotal", un "añadir otra fila", un "mostrar/ocultar contraseña". Lo clave es que **no dispara el submit**, evitando recargas accidentales.

> [!warning] El type por defecto de button es submit
> Un detalle crítico relacionado: un `<button>` **sin** `type` explícito dentro de un formulario actúa como `submit` y envía el formulario. Para un botón de acción con JS, hay que poner `type="button"` explícitamente (en `<input>` ya lo es; en `<button>` hay que declararlo). Olvidarlo provoca envíos y recargas inesperadas.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `type="button"` para acciones JS que no deben enviar el formulario.
> - Prefiere `<button type="button">` por su flexibilidad de contenido.
> - Para acciones fuera de formularios, igualmente un botón con `type="button"`.

## Errores comunes

> [!warning] Trampas
> - **Olvidar `type="button"`** en un `<button>` dentro de un form: envía el formulario sin querer.
> - **Usarlo para navegar**: para ir a otra página es un enlace [[01 Enlaces (a) | `<a>`]], no un botón.

## Notas relacionadas

- [[06 Botones (button)]] — la versión flexible y recomendada.
- [[20 input submit]] · [[21 input reset]] — los otros tipos de botón.
- [[01 Enlaces (a)]] — para navegar, en lugar de un botón.
