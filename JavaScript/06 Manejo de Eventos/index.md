---
title: Manejo de Eventos — Índice
aliases:
  - Eventos DOM
  - Event handling
  - manejo de eventos JavaScript
tags:
  - javascript
  - api/concepto
  - eventos
draft: false
---

# Manejo de Eventos

> [!definicion]
> El sistema de eventos es el mecanismo de interactividad del DOM. Cuando el usuario interactúa con la página (clic, teclado, scroll) o el navegador detecta un cambio de estado (carga completa, redimensionamiento, error), dispara un **evento** que recorre el árbol DOM. El código registra **listeners** en los elementos para reaccionar a esos eventos y manipular la interfaz en consecuencia.

El ciclo básico:

```js
// 1. Seleccionar el elemento
const btn = document.querySelector('#enviar');

// 2. Registrar un listener
btn.addEventListener('click', (e) => {
  // 3. El objeto Event llega al handler
  e.preventDefault();         // cancelar acción por defecto
  console.log(e.target);      // elemento que disparó el evento

  // 4. Actuar sobre el DOM
  btn.textContent = 'Enviado';
  btn.disabled = true;
});
```

## Contenido de la sección

| # | Nombre | Cubre |
|---|---|---|
| 01 | Tipos de Eventos | Categorías (ratón, teclado, formulario, ventana), jerarquía de interfaces |
| 02 | Registro de Eventos | addEventListener, opciones (once, passive, capture), removeEventListener, métodos legacy |
| 03 | El Objeto Event | target vs currentTarget, preventDefault, stopPropagation, propiedades específicas |
| 04 | Fases del Evento | Captura, target, burbujeo — orden de ejecución de listeners |
| 05 | Delegación de Eventos | Listener en ancestro, closest, elementos dinámicos, recetas |
| 06 | Eventos Personalizados | CustomEvent, dispatchEvent, comunicación entre componentes con detail |

## Cómo se relacionan los conceptos

El **registro** (sección 02) conecta un **listener** con un elemento. Cuando el evento se dispara, el motor construye el **objeto Event** (sección 03) y lo hace recorrer el árbol en las tres **fases** (sección 04). El burbujeo hace posible la **delegación** (sección 05). Y el mismo sistema de propagación se puede reutilizar con **eventos personalizados** (sección 06) para comunicación entre componentes.

## Notas relacionadas

- [[01 Tipos de Eventos/index|Tipos de Eventos]]
- [[02 Registro de Eventos/index|Registro de Eventos]]
- [[03 El Objeto Event/index|El Objeto Event]]
- [[04 Fases del Evento/index|Fases del Evento]]
- [[05 Delegación de Eventos|Delegación de Eventos]]
- [[06 Eventos Personalizados/index|Eventos Personalizados]]
