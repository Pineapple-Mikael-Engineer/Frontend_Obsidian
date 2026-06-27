---
title: removeAttribute y hasAttribute — Eliminar y comprobar atributos
aliases:
  - removeAttribute
  - hasAttribute
  - Element.removeAttribute
  - Element.hasAttribute
tags:
  - javascript
  - api/metodo
  - dom
objeto: Element
tipo: metodo
retorna: undefined | boolean
muta: true
asincrono: false
draft: false
order: 2
---

# removeAttribute y hasAttribute

> [!definicion]
> `elemento.removeAttribute(nombre)` elimina el atributo del elemento; no lanza error si el atributo no existe. `elemento.hasAttribute(nombre)` devuelve `true` si el atributo está presente en el elemento, independientemente de su valor (incluso si el valor es `""`).

```js
const btn = document.querySelector("button");

// Comprobar presencia
console.log(btn.hasAttribute("disabled")); // → true si <button disabled>

// Eliminar
btn.removeAttribute("disabled");
console.log(btn.hasAttribute("disabled")); // → false
```

## Uso correcto con atributos booleanos

Los atributos booleanos (`disabled`, `checked`, `required`, `readonly`, `hidden`, `selected`, `multiple`, `autofocus`, `autoplay`, `controls`, `loop`, `muted`, `open`, `reversed`) se rigen por presencia, no por valor. La forma correcta de activarlos y desactivarlos:

```js
// Deshabilitar — el valor del atributo no importa
btn.setAttribute("disabled", "");

// Habilitar — ELIMINAR el atributo, no poner "false"
btn.removeAttribute("disabled");

// TRAMPA — esto NO habilita el botón
btn.setAttribute("disabled", "false"); // el atributo existe → sigue deshabilitado
btn.setAttribute("disabled", "0");     // mismo problema
```

> [!warning]
> `removeAttribute("disabled")` es la única forma correcta de habilitar un elemento con atributos booleanos. Usar la propiedad IDL directa (`btn.disabled = false`) también funciona y es más idiomático para los atributos estándar.

## Toggle de atributo booleano

```js
function toggleAtributo(el, nombre) {
  if (el.hasAttribute(nombre)) {
    el.removeAttribute(nombre);
  } else {
    el.setAttribute(nombre, "");
  }
}

toggleAtributo(input, "readonly");
toggleAtributo(details, "open");
```

Para el atributo `class` (booleano en sentido de presencia de clase), `classList.toggle` es más adecuado.

## El NamedNodeMap: attributes

`elemento.attributes` devuelve un `NamedNodeMap` live con todos los atributos del elemento. Es iterable pero no es un array:

```js
const el = document.querySelector("#card");

// Iterar todos los atributos
for (const attr of el.attributes) {
  console.log(`${attr.name} = "${attr.value}"`);
}
// → id = "card"
// → class = "tarjeta activa"
// → data-id = "7"

// Número de atributos
console.log(el.attributes.length); // → 3

// Acceso por nombre
console.log(el.attributes.getNamedItem("class").value); // → "tarjeta activa"
```

## hasAttributes (sin nombre)

`elemento.hasAttributes()` (con s) devuelve `true` si el elemento tiene algún atributo. Distinto de `hasAttribute(nombre)` que comprueba uno concreto:

```js
document.createElement("div").hasAttributes(); // → false (elemento vacío)
document.querySelector("input").hasAttributes(); // → true (tiene type, al menos)
```

## Cómo funciona por dentro

`removeAttribute` busca el nodo de atributo en el `NamedNodeMap` y lo desconecta. La ausencia del atributo puede disparar cambios de estado en el elemento si existe un observer de mutación (`MutationObserver` con `attributes: true`). `hasAttribute` consulta el `NamedNodeMap` sin crear ni modificar nada — es una lectura pura.

> [!info]
> A diferencia de `getAttribute` que devuelve `null` para atributos inexistentes, `hasAttribute` devuelve `false`. Para verificar si un atributo tiene un valor específico: `el.getAttribute("role") === "button"`. Para verificar solo presencia: `el.hasAttribute("hidden")`.

## Notas relacionadas

- [[01 getAttribute y setAttribute]]
- [[03 Atributos Directos]]
- [[04 Modificar Atributos/index | Modificar Atributos]]
