---
title: Shadow DOM — encapsulación de árbol y estilos
aliases:
  - Shadow DOM
  - attachShadow
  - shadow root
tags:
  - javascript
  - api/concepto
  - browser
objeto: Web API
tipo: concepto
draft: false
order: 2
---

# Shadow DOM

> [!definicion]
> El **Shadow DOM** es un subárbol del DOM adjunto a un elemento host que está aislado del documento principal. Los estilos definidos dentro del Shadow DOM no afectan al exterior, y los estilos externos no penetran en él (salvo CSS Custom Properties y las partes expuestas con `::part`). Se crea con `element.attachShadow({ mode })` y retorna un `ShadowRoot` que actúa como el nodo raíz de ese árbol encapsulado.

```js
class MiCard extends HTMLElement {
  constructor() {
    super();
    const shadow = this.attachShadow({ mode: "open" });
    shadow.innerHTML = `
      <style>
        :host {
          display: block;
          border: 1px solid #ccc;
          border-radius: 8px;
          padding: 16px;
        }
        .titulo { font-weight: bold; font-size: 1.2rem; margin-bottom: 8px; }
      </style>
      <div class="titulo"><slot name="titulo"></slot></div>
      <slot></slot>
    `;
  }
}
customElements.define("mi-card", MiCard);
```

```html
<mi-card>
  <h2 slot="titulo">Encabezado</h2>
  <p>Este párrafo va al slot por defecto.</p>
</mi-card>
```

## `attachShadow()` — modos

```js
const shadow = element.attachShadow({ mode: "open" });   // accesible desde JS externo
const shadow = element.attachShadow({ mode: "closed" }); // oculto: shadowRoot retorna null
```

| Modo | `element.shadowRoot` desde fuera | Uso |
|---|---|---|
| `"open"` | Retorna el `ShadowRoot` | Web Components estándar, debugging sencillo |
| `"closed"` | Retorna `null` | Componentes que requieren encapsulación estricta (widgets de pago, etc.) |

`"closed"` no proporciona seguridad real — las DevTools siempre lo muestran y puede evitarse con técnicas de hooking. Su uso principal es señalar una API intencional, no ocultar código sensible.

## Slots — puntos de proyección del Light DOM

Los `<slot>` son puntos de inserción donde el contenido del Light DOM (los hijos del elemento host) se proyecta dentro del Shadow DOM. Los nodos proyectados permanecen en el Light DOM; solo se visualizan dentro del shadow.

```html
<!-- Dentro del Shadow DOM -->
<slot></slot>              <!-- slot por defecto: recibe todo el contenido sin slot= -->
<slot name="titulo"></slot> <!-- slot con nombre: recibe elementos con slot="titulo" -->
<slot name="pie">Pie por defecto</slot> <!-- contenido de fallback si el slot está vacío -->
```

```html
<!-- Light DOM del host -->
<mi-card>
  <h2 slot="titulo">Mi Título</h2>    <!-- → slot name="titulo" -->
  <p>Párrafo uno.</p>                 <!-- → slot por defecto -->
  <p>Párrafo dos.</p>                 <!-- → slot por defecto -->
</mi-card>
```

`slot.assignedNodes()` y `slot.assignedElements()` retornan los nodos proyectados en un slot.

## CSS desde dentro del Shadow DOM

```css
/* El elemento host en sí */
:host { display: block; }
:host(.activo) { border-color: blue; }       /* cuando el host tiene la clase "activo" */
:host-context(.tema-oscuro) { color: white; } /* cuando un ancestro tiene .tema-oscuro */

/* Elementos en slots (Light DOM proyectado) */
::slotted(p) { margin: 0; }           /* párrafos en cualquier slot */
::slotted([slot="titulo"]) { ... }    /* elemento con slot="titulo" */

/* Elementos internos normales */
.interno { color: red; } /* solo afecta al shadow tree */
```

## CSS desde fuera — traversal permitido

Los estilos externos no penetran el Shadow DOM, con dos excepciones:

**CSS Custom Properties** — las variables CSS atraviesan el Shadow boundary. La API pública de estilos de un Web Component se define mediante custom properties:

```css
/* Desde fuera */
mi-card { --color-borde: #4f46e5; --radio: 12px; }

/* Dentro del Shadow DOM */
:host { border: 1px solid var(--color-borde, #ccc); border-radius: var(--radio, 8px); }
```

**`::part()`** — el shadow puede exponer partes estilizables con el atributo `part`:

```html
<!-- Dentro del Shadow DOM -->
<button part="boton-accion">Acción</button>
```

```css
/* Desde fuera */
mi-card::part(boton-accion) { background: blue; color: white; }
```

## Acceso al Shadow Root

```js
const shadow = elemento.shadowRoot;          // solo si mode: "open"
shadow.querySelector(".titulo");             // buscar dentro del shadow
shadow.innerHTML = "...";                    // reemplazar contenido

// Evento: slotchange — cuando los nodos proyectados cambian
shadow.querySelector("slot").addEventListener("slotchange", e => {
  const nodos = e.target.assignedNodes();
  console.log("Nodos proyectados:", nodos);
});
```

## Cómo funciona por dentro

El Shadow DOM crea un árbol alternativo adjunto al elemento host pero separado del árbol principal del documento. El browser mantiene dos árboles: el **Light DOM** (el árbol normal del documento) y el **Shadow Tree**. Al renderizar, el compositor construye un **Flat Tree** que combina ambos, proyectando el Light DOM en los slots del Shadow Tree. Los selectores CSS del documento principal no pueden alcanzar el Shadow Tree porque el selector matching ocurre en el Light DOM; los selectores del Shadow Tree solo ven su propio árbol.

Los eventos que burbujean desde dentro del Shadow DOM tienen su `target` re-mapeado al elemento host cuando atraviesan el shadow boundary, preservando la encapsulación: los listeners externos no pueden distinguir qué nodo interno disparó el evento.

> [!tip]
> Para estilos globales que sí deben penetrar (tipografía base, reset de CSS), la solución correcta es usar CSS Custom Properties como variables de diseño (`--font-family`, `--font-size-base`) que el componente consume en su Shadow DOM. Esto permite que los componentes sean temizables sin romper la encapsulación.

> [!warning]
> `document.querySelector()` no encuentra elementos dentro de un Shadow DOM. Para acceder a ellos desde fuera, se necesita primero obtener el `shadowRoot` del host (`element.shadowRoot`) y luego llamar a `shadowRoot.querySelector()`. Si el shadow es `"closed"`, no hay acceso programático externo.

## Notas relacionadas

- [[index|Web Components]] — visión general de las tres tecnologías.
- [[01 Custom Elements]] — el Shadow DOM se crea en el `constructor()` del Custom Element.
- [[03 HTML Templates]] — el `<template>` se clona y se adjunta al Shadow Root en el constructor.
