---
title: Manipulación del DOM
aliases:
  - DOM
  - Document Object Model
  - manipulación del DOM
tags:
  - javascript
  - api/concepto
  - dom
draft: false
---

# Manipulación del DOM

> [!definicion]
> El **DOM** (Document Object Model) es la representación en memoria del documento HTML como un árbol de nodos. El navegador construye este árbol al parsear el HTML; JavaScript puede leerlo y modificarlo en tiempo real a través de la Web API, actualizando la pantalla en consecuencia.

El ciclo básico de cualquier operación DOM:

```js
// 1. Seleccionar
const el = document.querySelector('.tarjeta');

// 2. Leer o modificar
el.textContent = 'Nuevo texto';
el.classList.add('activo');

// 3. Insertar o eliminar si es necesario
const nuevo = document.createElement('p');
nuevo.textContent = 'Párrafo añadido';
el.appendChild(nuevo);
```

## Subsecciones

| # | Nombre | Cubre |
|---|---|---|
| 01 | Selección de Elementos | `getElementById`, `querySelector`, `querySelectorAll`, `HTMLCollection` vs `NodeList` |
| 02 | Recorrer el DOM | `parentNode`, `children`, `nextSibling`, navegación por el árbol |
| 03 | Modificar Contenido | `textContent`, `innerHTML`, `innerText`, `insertAdjacentHTML` |
| 04 | Modificar Atributos | `getAttribute`, `setAttribute`, `dataset`, `classList` |
| 05 | Modificar Estilos | `style`, `getComputedStyle`, variables CSS desde JS |
| 06 | Crear e Insertar Elementos | `createElement`, `appendChild`, `insertBefore`, `append`, `prepend` |
| 07 | Eliminar Elementos | `removeChild`, `remove`, `replaceWith`, `replaceChild` |
| 08 | Dimensiones y Posiciones | `offsetWidth`, `clientWidth`, `scrollWidth`, `getBoundingClientRect`, scroll |
| 09 | Actualizaciones Eficientes | `DocumentFragment`, `requestAnimationFrame`, reflows y repaints |

## Organización del árbol DOM

El árbol tiene tres tipos de nodo relevantes en la práctica:

- **Element nodes** — las etiquetas HTML (`<div>`, `<p>`, `<input>`…). Tienen atributos, clases y estilos.
- **Text nodes** — el texto dentro de los elementos. Accesibles via `textContent` en el elemento padre.
- **Document node** — la raíz; punto de entrada para la mayoría de métodos (`document.querySelector`, `document.createElement`…).

## Notas relacionadas

- [[01 Selección de Elementos/index|Selección de Elementos]]
- [[02 Recorrer el DOM/index|Recorrer el DOM]]
- [[03 Modificar Contenido/index|Modificar Contenido]]
- [[04 Modificar Atributos/index|Modificar Atributos]]
- [[05 Modificar Estilos/index|Modificar Estilos]]
- [[06 Crear e Insertar Elementos/index|Crear e Insertar Elementos]]
- [[07 Eliminar Elementos/index|Eliminar Elementos]]
- [[08 Dimensiones y Posiciones/index|Dimensiones y Posiciones]]
- [[09 Actualizaciones Eficientes/index|Actualizaciones Eficientes]]
