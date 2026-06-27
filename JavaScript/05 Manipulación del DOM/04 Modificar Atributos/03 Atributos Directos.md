---
title: Atributos Directos — Propiedades IDL del elemento DOM
aliases:
  - Atributos Directos
  - Propiedades IDL
  - IDL attributes
tags:
  - javascript
  - api/concepto
  - dom
objeto: HTMLElement
tipo: concepto
muta: true
asincrono: false
draft: false
order: 3
---

# Atributos Directos

> [!definicion]
> Los atributos directos son **propiedades JS del objeto DOM** definidas en las interfaces IDL (`HTMLInputElement`, `HTMLAnchorElement`, etc.) que corresponden a atributos HTML estándar: `el.id`, `el.className`, `el.href`, `el.src`, `el.value`, `el.checked`, `el.disabled`, `el.type`, `el.name`. Son más ergonómicas y tipadas que `getAttribute`/`setAttribute`, pero su valor puede divergir del atributo HTML subyacente.

```js
const input = document.querySelector('input[type="email"]');

// Leer — tipado, no siempre string
console.log(input.type);     // → "email"
console.log(input.disabled); // → false (boolean, no string)
console.log(input.value);    // → valor actual del campo

// Escribir
input.placeholder = "tu@email.com";
input.disabled = true;  // equivale a setAttribute("disabled", "")
input.disabled = false; // equivale a removeAttribute("disabled")
```

## Atributos más usados y sus tipos

| Propiedad | Tipo JS | Elemento(s) | Observación |
|-----------|---------|-------------|-------------|
| `el.id` | `string` | todos | Reflejo bidireccional del atributo `id` |
| `el.className` | `string` | todos | String completo de clases; ver `classList` |
| `el.hidden` | `boolean` | todos | Refleja atributo `hidden` |
| `el.tabIndex` | `number` | todos | Refleja `tabindex` |
| `el.href` | `string` (URL absoluta) | `<a>`, `<link>` | Diverge del atributo |
| `el.src` | `string` (URL absoluta) | `<img>`, `<script>`, `<iframe>` | Diverge del atributo |
| `el.value` | `string` | `<input>`, `<textarea>`, `<select>` | Estado actual, no inicial |
| `el.checked` | `boolean` | `<input type="checkbox/radio">` | Estado actual |
| `el.disabled` | `boolean` | form elements | Activa/desactiva sin setAttr |
| `el.required` | `boolean` | form elements | |
| `el.type` | `string` | `<input>`, `<button>` | Normalizado a lowercase |
| `el.name` | `string` | form elements | |

## La divergencia atributo / propiedad

La diferencia más importante: para `value` y `checked`, el atributo HTML es el **valor inicial**, y la propiedad IDL es el **valor actual** (modificable por el usuario o por JS):

```js
// HTML: <input type="text" value="inicial">
const input = document.querySelector("input");

input.value = "modificado por JS"; // cambia la propiedad, no el atributo
console.log(input.value);                // → "modificado por JS"
console.log(input.getAttribute("value")); // → "inicial" — el atributo no cambió

// Para sincronizar el atributo con la propiedad:
input.setAttribute("value", input.value);
// Ahora input.getAttribute("value") === "modificado por JS"
```

> [!warning]
> Al hacer `input.value = ""` para limpiar un campo, el atributo `value` del HTML no se modifica — solo la propiedad. Si después se hace `input.form.reset()`, el campo vuelve al valor del atributo (el inicial del markup), no al vacío.

## href y src: URL resuelta vs literal

`el.href` y `el.src` devuelven la URL **absoluta** resuelta contra el documento base, no el valor literal del atributo:

```js
// HTML: <a href="./pagina.html">Enlace</a>
const a = document.querySelector("a");

console.log(a.getAttribute("href")); // → "./pagina.html" — literal del atributo
console.log(a.href);                 // → "https://ejemplo.com/directorio/pagina.html"

// Para conservar el valor literal al leer:
a.getAttribute("href"); // usar getAttribute, no la propiedad
```

## className vs classList

`el.className` es el string completo de clases. Para manipular clases individuales, [[04 classList]] es la API apropiada:

```js
const el = document.querySelector(".card");

// className — reemplaza todo el string de clases
el.className = "card activo destacado"; // borra las clases previas y las reemplaza

// classList — API moderna para clases individuales (no destructiva)
el.classList.add("activo");
el.classList.remove("destacado");
```

## Cómo funciona por dentro

Las propiedades IDL están definidas en las interfaces WebIDL de los elementos (especificación HTML). Internamente son getters/setters en el prototipo del constructor (`HTMLInputElement.prototype`, `HTMLAnchorElement.prototype`, etc.). Algunos son **reflejos directos** del atributo (sincronización bidireccional, como `id`), otros son **propiedades de estado** que inicialmente leen el atributo pero luego viven de forma independiente (`value`, `checked`), y otros aplican transformaciones (`href` → URL absoluta).

> [!info]
> `typeof input.disabled` es `"boolean"`, no `"string"`. Las propiedades IDL booleanas se asignan con `true`/`false` y el motor se encarga de reflejarlo en el atributo HTML subyacente (add/remove del atributo).

## Notas relacionadas

- [[01 getAttribute y setAttribute]]
- [[02 removeAttribute y hasAttribute]]
- [[04 classList]]
- [[04 Modificar Atributos/index | Modificar Atributos]]
