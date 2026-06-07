---
title: getAttribute y setAttribute — Leer y escribir atributos como string
aliases:
  - getAttribute
  - setAttribute
  - Element.getAttribute
  - Element.setAttribute
tags:
  - javascript
  - api/metodo
  - dom
objeto: Element
tipo: metodo
retorna: string | null
muta: true
asincrono: false
draft: false
---

# getAttribute y setAttribute

> [!definicion]
> `elemento.getAttribute(nombre)` devuelve el valor del atributo HTML como **string**, o `null` si el atributo no existe. `elemento.setAttribute(nombre, valor)` establece el atributo con el valor dado (convertido a string), creándolo si no existe. Ambos operan sobre el atributo HTML, no sobre la propiedad IDL del objeto.

```js
const input = document.querySelector('input[type="text"]');

// Leer
console.log(input.getAttribute("placeholder")); // → "Buscar..."
console.log(input.getAttribute("noexiste"));    // → null

// Escribir
input.setAttribute("placeholder", "Nombre completo");
input.setAttribute("maxlength", "50"); // 50 se convierte a string "50"

// Crear atributo nuevo
input.setAttribute("data-validado", "true");
```

## Diferencia con propiedades directas

La distinción entre atributo HTML y propiedad IDL es relevante cuando el usuario modifica el elemento:

```js
const input = document.querySelector('input[type="text"]');
// HTML inicial: <input type="text" value="inicial">

// El usuario escribe "modificado" en el campo

console.log(input.getAttribute("value")); // → "inicial" — valor del atributo HTML
console.log(input.value);                 // → "modificado" — valor actual (IDL)

// Lo mismo con checked
const checkbox = document.querySelector('input[type="checkbox"]');
// HTML inicial: <input type="checkbox" checked>
checkbox.click(); // el usuario lo desmarca

console.log(checkbox.getAttribute("checked")); // → "" — el atributo sigue presente
console.log(checkbox.checked);                 // → false — estado actual
```

> [!info]
> `getAttribute` devuelve el **valor inicial del markup**, que es independiente del estado actual del elemento. Las propiedades IDL (como `input.value`, `input.checked`) reflejan el estado en tiempo real y son lo que se debe leer para conocer el valor actual.

## Atributos booleanos

Los atributos booleanos (`disabled`, `checked`, `required`, `readonly`, `hidden`) funcionan por presencia: la presencia del atributo activa el estado; su valor literal no importa.

```js
// Habilitar y deshabilitar correctamente
btn.setAttribute("disabled", "");    // deshabilita — valor vacío es válido
btn.setAttribute("disabled", "false"); // ¡sigue deshabilitado! — el atributo está presente

// Para habilitar, eliminar el atributo:
btn.removeAttribute("disabled"); // ver nota 02

// Las propiedades IDL son más directas para booleanos:
btn.disabled = true;
btn.disabled = false;
```

## Atributos con namespace (SVG / XML)

Para atributos en namespaces específicos (frecuente en SVG):

```js
const rect = document.querySelector("rect");

// SVG — algunos atributos requieren namespace explícito
rect.getAttributeNS("http://www.w3.org/1999/xlink", "href");
rect.setAttributeNS(null, "fill", "red"); // null = sin namespace (HTML/SVG genérico)
```

## Recetas comunes

```js
// Leer data-* (alternativa a dataset)
const id = el.getAttribute("data-user-id"); // → "42" (string)

// Forzar re-validación de un form field
campo.setAttribute("required", "");
campo.removeAttribute("required");

// Añadir atributo ARIA dinámico
boton.setAttribute("aria-expanded", String(estaAbierto));
boton.setAttribute("aria-controls", "menu-desplegable");
```

## Cómo funciona por dentro

Los atributos HTML son pares nombre-valor de tipo string almacenados en el `NamedNodeMap` del elemento (`el.attributes`). Son distintos de las propiedades del objeto JS — estas últimas se definen en las interfaces IDL (como `HTMLInputElement`) y se sincronizan con los atributos mediante getters/setters en el prototipo. Algunos atributos y propiedades están sincronizados bidireccionalmente (`id`, `className`); otros solo en una dirección (`value`, `checked`); otros no están sincronizados (`href` difiere entre atributo literal y propiedad URL absoluta).

> [!warning]
> `setAttribute` convierte cualquier valor a string. `setAttribute("disabled", false)` asigna la string `"false"`, y el elemento queda deshabilitado porque el atributo existe. Para propiedades booleanas, usar las propiedades directas (`el.disabled = false`) o `removeAttribute`.

## Notas relacionadas

- [[02 removeAttribute y hasAttribute]]
- [[03 Atributos Directos]]
- [[05 dataset]]
- [[04 Modificar Atributos/index | Modificar Atributos]]
