---
title: Custom Elements — definir nuevas etiquetas HTML
aliases:
  - Custom Elements
  - customElements.define
  - HTMLElement custom
tags:
  - javascript
  - api/clase
  - browser
objeto: Web API
tipo: clase
draft: false
order: 1
---

# Custom Elements

> [!definicion]
> Los **Custom Elements** permiten registrar nuevas etiquetas HTML con comportamiento JS propio. Una clase que extiende `HTMLElement` (o un elemento específico) se registra con `customElements.define("mi-elemento", MiClase)`. A partir de ese momento, `<mi-elemento>` en el HTML o `document.createElement("mi-elemento")` produce una instancia de esa clase, con acceso al ciclo de vida estándar del DOM.

```js
class MiBoton extends HTMLElement {
  static observedAttributes = ["disabled", "variant"];

  constructor() {
    super();
    // Solo: llamar super() y configurar Shadow DOM.
    // NO manipular Light DOM aquí — el elemento aún no está en el documento.
  }

  connectedCallback() {
    this.innerHTML = `<button class="btn btn--${this.getAttribute("variant") || "default"}">
      ${this.getAttribute("label") || "Botón"}
    </button>`;
    this.querySelector("button").addEventListener("click", this._onClick.bind(this));
  }

  disconnectedCallback() {
    // Limpiar listeners o timers para evitar memory leaks
    this.querySelector("button")?.removeEventListener("click", this._onClick);
  }

  attributeChangedCallback(nombre, valorAnterior, valorNuevo) {
    if (!this.isConnected) return; // el DOM aún no está disponible
    if (nombre === "disabled") {
      this.querySelector("button").disabled = valorNuevo !== null;
    }
  }

  _onClick(e) {
    this.dispatchEvent(new CustomEvent("mi-click", { bubbles: true, detail: { origen: this } }));
  }
}

customElements.define("mi-boton", MiBoton);
```

```html
<mi-boton label="Enviar" variant="primary"></mi-boton>
```

## Ciclo de vida

| Callback | Cuándo se invoca |
|---|---|
| `constructor()` | Al crear la instancia (con `new` o al parsear el HTML) |
| `connectedCallback()` | Al insertar el elemento en el documento |
| `disconnectedCallback()` | Al eliminar el elemento del documento |
| `attributeChangedCallback(nombre, antes, después)` | Al cambiar un atributo listado en `observedAttributes` |
| `adoptedCallback()` | Al mover el elemento a otro `document` (p. ej. un iframe) |

El orden garantizado: `constructor` → `attributeChangedCallback` (si hay atributos en el HTML) → `connectedCallback`. `disconnectedCallback` y `adoptedCallback` pueden no llamarse si el elemento se elimina sin pasar por el GC normalmente.

## Reglas del constructor

El constructor solo puede:
1. Llamar a `super()` (obligatorio, debe ser la primera instrucción).
2. Crear y adjuntar un Shadow DOM (`this.attachShadow()`).
3. Configurar listeners en el propio Shadow DOM.

No debe acceder a atributos del elemento, manipular Light DOM, o asumir que el elemento tiene hijos. Todo eso va en `connectedCallback()`.

## `observedAttributes` y `attributeChangedCallback`

```js
class MiElemento extends HTMLElement {
  static observedAttributes = ["color", "tamaño"]; // solo estos atributos disparan el callback

  attributeChangedCallback(nombre, valorAnterior, valorNuevo) {
    // valorNuevo es null cuando el atributo es eliminado
    this.style.color = this.getAttribute("color") || "inherit";
  }
}
```

Solo los atributos listados en el getter estático `observedAttributes` disparan `attributeChangedCallback`. Atributos no listados pueden leerse con `getAttribute()` pero no generan notificaciones.

## API de `customElements`

```js
customElements.define("mi-tag", MiClase);               // registrar
customElements.define("mi-tag", MiClase, { extends: "button" }); // Customized Built-in

customElements.get("mi-tag");                            // → MiClase (o undefined)
await customElements.whenDefined("mi-tag");              // Promise — esperar a que se defina
customElements.upgrade(elemento);                        // actualizar elemento creado antes de definir
```

## Customized Built-in Elements

Extienden un elemento HTML existente en lugar de `HTMLElement`, heredando su semántica y accesibilidad:

```js
class MiInput extends HTMLInputElement {
  connectedCallback() {
    this.placeholder = this.getAttribute("placeholder") || "Escribe aquí…";
  }
}
customElements.define("mi-input", MiInput, { extends: "input" });
```

```html
<input is="mi-input" placeholder="Nombre">
```

> [!warning]
> Customized Built-in Elements **no están soportados en Safari** (sin polyfill). Apple se opone a la propuesta en el estándar. Para compatibilidad universal, preferir Autonomous Custom Elements (extensiones directas de `HTMLElement`) con Shadow DOM para la estructura interna.

## Cómo funciona por dentro

Al parsear HTML, el browser crea instancias de `HTMLElement` genéricas para las etiquetas desconocidas. Cuando se llama a `customElements.define()`, el browser hace un **upgrade** de todos los elementos existentes con ese nombre: destruye la instancia genérica y crea una de la clase registrada, invocando `constructor` → `attributeChangedCallback` → `connectedCallback`. Los elementos creados después de la definición se instancian directamente como la clase registrada.

`customElements.whenDefined("mi-tag")` retorna una Promise que se resuelve cuando el upgrade termina, útil para evitar FOUC (Flash of Unstyled Content) con Custom Elements no definidos aún.

> [!tip]
> Para comunicar hacia afuera, usar `CustomEvent` con `bubbles: true` — los eventos burbujean por el DOM igual que los eventos nativos. Para comunicar hacia adentro (de padre a hijo), usar atributos o propiedades JS directamente (`elemento.propiedad = valor`).

> [!warning]
> Si se accede a `this.children`, `this.getAttribute()` o se manipula `this.innerHTML` dentro del `constructor`, el browser lanza `DOMException: Failed to construct. The result must not have children`. Todo acceso al Light DOM del elemento debe ocurrir en `connectedCallback` o posterior.

## Notas relacionadas

- [[index|Web Components]] — visión general de las tres tecnologías.
- [[02 Shadow DOM]] — encapsulación de estilos y árbol interno del componente.
- [[03 HTML Templates]] — usar `<template>` para definir el markup interno del componente.
