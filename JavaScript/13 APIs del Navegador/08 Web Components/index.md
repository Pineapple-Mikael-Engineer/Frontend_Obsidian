---
title: Web Components — elementos HTML personalizados y encapsulados
aliases:
  - Web Components
  - Custom Elements
  - Shadow DOM
  - HTML Templates
tags:
  - javascript
  - api/concepto
  - browser
draft: false
order: 7
---

# Web Components

> [!definicion]
> Los **Web Components** son un conjunto de estándares del browser que permiten crear elementos HTML personalizados, reutilizables y encapsulados. Se componen de tres tecnologías ortogonales: **Custom Elements** (definir nuevas etiquetas con comportamiento JS), **Shadow DOM** (aislar el árbol interno y los estilos) y **HTML Templates** (`<template>` para fragmentos HTML lazy que se clonan cuando se necesitan). Juntas permiten construir componentes portables sin dependencias de framework.

```html
<!-- Uso de un Web Component definido con las tres tecnologías -->
<mi-card>
  <h2 slot="titulo">Título de la tarjeta</h2>
  <p>Contenido en el slot por defecto.</p>
</mi-card>
```

```js
// Definición mínima que combina las tres tecnologías
class MiCard extends HTMLElement {
  constructor() {
    super();
    const tmpl = document.getElementById("mi-card-tmpl").content.cloneNode(true);
    this.attachShadow({ mode: "open" }).appendChild(tmpl);
  }
}
customElements.define("mi-card", MiCard);
```

## Las tres tecnologías

| Tecnología | API principal | Responsabilidad |
|---|---|---|
| Custom Elements | `customElements.define()` | Registrar el elemento y gestionar su ciclo de vida |
| Shadow DOM | `element.attachShadow()` | Encapsular el árbol interno y aislar los estilos |
| HTML Templates | `<template>`, `<slot>` | Definir el markup reutilizable que se clona al crear cada instancia |

Las tres son independientes: se puede usar `<template>` sin Custom Elements, o crear un Shadow DOM sin una clase propia. La combinación es el patrón canónico de un Web Component completo.

## Cuándo usar Web Components

Web Components son una buena elección cuando se necesita:
- un componente reutilizable en proyectos con distintos frameworks (o sin ninguno),
- encapsulación real de estilos (sin CSS Modules ni convenciones de naming),
- un widget distribuible como paquete npm que funcione en cualquier HTML.

Para apps con un único framework (React, Vue, Angular), los componentes nativos del framework suelen ser más ergonómicos; los Web Components no son un reemplazo universal.

## Notas de esta sección

- [[01 Custom Elements]] — definir etiquetas HTML personalizadas y gestionar el ciclo de vida.
- [[02 Shadow DOM]] — encapsulación de árbol y estilos: slots, `:host`, `::part`.
- [[03 HTML Templates]] — `<template>` y `cloneNode` para markup reutilizable sin parsing repetido.

> [!tip]
> El orden de aprendizaje recomendado es Custom Elements → HTML Templates → Shadow DOM. Los primeros dos son independientes y más sencillos; Shadow DOM introduce el modelo de encapsulación que combina los tres.

> [!warning]
> Los Custom Elements deben contener un guión en el nombre (`mi-boton`, no `miBoton` ni `boton`). Sin guión, `customElements.define()` lanza `DOMException`. Esta restricción es intencional para evitar colisiones con futuros elementos HTML estándar.

## Notas relacionadas

- [[../06 Observers/index|Observers]] — `MutationObserver` es útil para reaccionar a cambios dentro de un Custom Element cuando no se controla el Shadow DOM.
- [[../../05 Manipulación del DOM/index|Manipulación del DOM]] — los Web Components extienden el DOM; conocer `appendChild`, `querySelector` y `cloneNode` es necesario para implementarlos.
- [[../../10 Módulos y Organización/index|Módulos y Organización]] — los Web Components se distribuyen como módulos ES; `import` de la definición registra el elemento en `customElements`.
