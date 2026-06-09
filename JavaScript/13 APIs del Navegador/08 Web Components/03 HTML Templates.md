---
title: HTML Templates — markup reutilizable con <template>
aliases:
  - HTML Templates
  - template element
  - DocumentFragment
  - cloneNode
tags:
  - javascript
  - api/concepto
  - browser
objeto: Web API
tipo: concepto
draft: false
---

# HTML Templates

> [!definicion]
> El elemento `<template>` contiene HTML que el browser parsea pero **no renderiza** hasta que se clona e inserta explícitamente con JS. Su contenido vive en un `DocumentFragment` accesible como `template.content`. A diferencia de un `<div>` oculto, el contenido del template no genera requests de recursos (imágenes, scripts), no ejecuta código y no forma parte del árbol de layout — solo existe como un árbol parseado en memoria. `cloneNode(true)` produce una copia independiente lista para insertar.

```html
<template id="tarjeta-tmpl">
  <article class="tarjeta">
    <h3 class="titulo"></h3>
    <p class="descripcion"></p>
    <footer class="pie"></footer>
  </article>
</template>
```

```js
const tmpl = document.getElementById("tarjeta-tmpl");

function crearTarjeta({ titulo, descripcion, pie }) {
  const clone = tmpl.content.cloneNode(true); // deep clone del DocumentFragment
  clone.querySelector(".titulo").textContent = titulo;
  clone.querySelector(".descripcion").textContent = descripcion;
  clone.querySelector(".pie").textContent = pie;
  return clone; // DocumentFragment listo para insertar
}

const frag = crearTarjeta({ titulo: "Card 1", descripcion: "Contenido", pie: "Autor" });
document.getElementById("lista").appendChild(frag);
```

## `template.content` — DocumentFragment

`template.content` es un `DocumentFragment` — un nodo ligero que puede contener múltiples hijos pero no forma parte del árbol del documento hasta que se inserta. Al adjuntarlo con `appendChild` o `append`, el `DocumentFragment` se vacía (sus hijos pasan al destino), por eso es necesario clonar antes de insertar si se van a crear múltiples instancias.

```js
const frag = tmpl.content.cloneNode(true);   // deep clone: incluye todos los descendientes
const shallow = tmpl.content.cloneNode(false); // shallow: solo el DocumentFragment vacío (raramente útil)
```

## Por qué `<template>` sobre `innerHTML`

| Criterio | `<template>` + `cloneNode` | `innerHTML` |
|---|---|---|
| Parsing | Una sola vez al cargar el HTML | En cada inserción |
| Rendimiento con N instancias | O(1) parse + O(N) clone | O(N) parse |
| Recursos (imágenes, iframes) | No se cargan hasta la inserción | Pueden dispararse al parsear |
| Seguridad | El template no ejecuta scripts | `innerHTML` ejecuta scripts en ciertos contextos |
| Tipo retornado | `DocumentFragment` | `string` → nodo en el DOM |

Para componentes que se instancian muchas veces (listas largas, filas de tabla), el template se parsea una sola vez y cada instancia se crea clonando el `DocumentFragment`, que es significativamente más rápido que re-parsear HTML en cada iteración.

## Integración con Custom Elements y Shadow DOM

El patrón canónico de un Web Component combina las tres tecnologías: el `<template>` define la estructura, y el `constructor()` del Custom Element lo clona y lo adjunta al Shadow Root:

```html
<template id="mi-boton-tmpl">
  <style>
    :host { display: inline-block; }
    button { padding: 8px 16px; border-radius: 4px; cursor: pointer; }
  </style>
  <button part="boton"><slot></slot></button>
</template>
```

```js
class MiBoton extends HTMLElement {
  constructor() {
    super();
    const tmpl = document.getElementById("mi-boton-tmpl");
    const shadow = this.attachShadow({ mode: "open" });
    shadow.appendChild(tmpl.content.cloneNode(true));
  }
}
customElements.define("mi-boton", MiBoton);
```

```html
<mi-boton>Enviar</mi-boton>
```

Para componentes distribuidos como módulos ES (sin un HTML externo que defina el `<template>`), el template se crea y parsea en JS una sola vez como variable de módulo:

```js
// Patrón de template-en-módulo (sin depender de un <template> en el HTML)
const tmpl = document.createElement("template");
tmpl.innerHTML = `
  <style>:host { display: block; }</style>
  <slot></slot>
`;

class MiElemento extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: "open" }).appendChild(tmpl.content.cloneNode(true));
  }
}
```

El `innerHTML` de `tmpl` se parsea solo una vez cuando se evalúa el módulo, y todas las instancias clonan el mismo `DocumentFragment`.

## `<slot>` dentro de `<template>`

Los elementos `<slot>` dentro de un `<template>` solo actúan como puntos de proyección cuando el template se inserta en un Shadow DOM. En un template insertado en el Light DOM, los `<slot>` son elementos HTML normales (sin comportamiento especial).

```html
<template id="con-slots">
  <div class="cabecera"><slot name="cabecera">Sin cabecera</slot></div>
  <main><slot></slot></main>
</template>
```

El funcionamiento de `<slot>` se describe en [[02 Shadow DOM]].

## Cómo funciona por dentro

El browser parsea el contenido del `<template>` en un documento inerte (sin contexto de browsing) durante el parsing del HTML inicial. Esto garantiza que el contenido está disponible como árbol DOM parseado desde el primer ciclo de rendering, sin coste adicional. El `DocumentFragment` resultante vive como propiedad `content` del elemento `HTMLTemplateElement` en memoria, sin estar conectado al árbol principal.

> [!tip]
> Al definir un Web Component como módulo ES, declarar el template como constante de módulo (fuera de la clase) garantiza que el parsing del innerHTML ocurre exactamente una vez por módulo, independientemente de cuántas instancias se creen. La alternativa de crear el template dentro del constructor parsea el HTML en cada instanciación, anulando la ventaja de rendimiento.

> [!warning]
> `tmpl.content.cloneNode(true)` produce una copia independiente, pero si el contenido del template contiene referencias a variables externas o closures (no es posible en HTML puro, pero sí en templates generados con JS), las copias comparten las referencias al mismo objeto. Las modificaciones a propiedades compartidas afectan a todas las instancias. Para estado por instancia, inicializarlo en `connectedCallback()`, no en el template.

## Notas relacionadas

- [[index|Web Components]] — visión general de las tres tecnologías.
- [[01 Custom Elements]] — el template se usa en el constructor del Custom Element.
- [[02 Shadow DOM]] — los slots del template cobran sentido en el contexto de Shadow DOM.
- [[../../05 Manipulación del DOM/index|Manipulación del DOM]] — `cloneNode`, `DocumentFragment` y `appendChild` son las operaciones DOM que usa esta API.
