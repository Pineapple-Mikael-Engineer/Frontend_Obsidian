---
title: JavaScript — Documentación de referencia
aliases:
  - JavaScript
  - JS
tags:
  - javascript
  - api/concepto
draft: false
order: 3
---

# JavaScript

> [!definicion]
> **JavaScript** es un lenguaje de scripting interpretado, dinámicamente tipado y single-threaded, que ejecuta código de forma no bloqueante mediante un **event loop** y una cola de tareas. Nació en 1995 como lenguaje de scripts embebido en el navegador, pero evolucionó hasta convertirse en una plataforma de propósito general disponible en el servidor (Node.js, Deno), en dispositivos móviles (React Native) y en entornos embebidos, siendo hoy el lenguaje más ubicuo del ecosistema web.

El lenguaje se articula en torno a tres pilares que coexisten: **funciones de primera clase** (closures, callbacks, composición), un **modelo de objetos prototípico** (con sintaxis de clases encima) y una **concurrencia cooperativa** basada en Promesas y `async`/`await`. Dominar JavaScript supone entender cómo estos tres pilares interactúan en el contexto del navegador y del runtime de servidor.

```js
// Firma del lenguaje: closure + async/await + DOM en acción
const crearFetcher = (baseURL) => {
  // closure sobre baseURL
  return async (ruta) => {
    const resp = await fetch(`${baseURL}${ruta}`);
    if (!resp.ok) throw new Error(`HTTP ${resp.status}`);
    return resp.json();
  };
};

const api = crearFetcher('https://api.ejemplo.com');

document.querySelector('#btn').addEventListener('click', async () => {
  const datos = await api('/usuarios');
  document.querySelector('#lista').textContent = JSON.stringify(datos);
});
```

## Subsecciones

| # | Nombre | Contenido principal |
|---|---|---|
| 01 | Programación Procedimental | Variables, tipos, operadores, control de flujo, funciones, scope, hoisting y closures — el núcleo imperativo del lenguaje |
| 02 | Programación Orientada a Objetos | Prototipos, cadena de herencia, sintaxis de clases ES6+, encapsulación, polimorfismo y patrones OOP en JS |
| 03 | Programación Funcional | Funciones puras, inmutabilidad, `map`/`filter`/`reduce`, composición, currying, mónadas y técnicas FP aplicadas |
| 04 | Estructuras de Datos | Arrays, objetos, `Map`, `Set`, `WeakMap`, `WeakSet`, iteradores, generadores y algoritmos de manipulación |
| 05 | Manipulación del DOM | Selección, traversal, creación y modificación de nodos, `innerHTML` vs `textContent`, y renderizado eficiente |
| 06 | Manejo de Eventos | Modelo de eventos del navegador, bubbling/capturing, delegación, `CustomEvent`, y gestión del ciclo de vida |
| 07 | Programación Asíncrona | Event loop, callbacks, Promesas, `async`/`await`, `Promise.all`/`race`, y Web Workers |
| 08 | Comunicación con APIs | `fetch`, `XMLHttpRequest`, REST, GraphQL, WebSockets, SSE y manejo de errores de red |
| 09 | Almacenamiento en Cliente | `localStorage`, `sessionStorage`, `IndexedDB`, cookies, Cache API y estrategias de persistencia offline |
| 10 | Módulos y Organización | ES Modules (`import`/`export`), CommonJS, bundlers (Vite, Webpack), tree-shaking y arquitectura de proyecto |
| 11 | Manejo de Errores y Depuración | `try`/`catch`/`finally`, errores personalizados, DevTools, breakpoints, profiling y estrategias de logging |
| 12 | Patrones y Buenas Prácticas | Patrones de diseño (Factory, Observer, Module, Singleton), principios SOLID en JS, y code smells a evitar |
| 13 | APIs del Navegador | Web APIs nativas: Intersection Observer, ResizeObserver, History API, Web Components, Canvas, WebGL y más |

## Arco de aprendizaje

Las 13 secciones siguen una progresión deliberada que conviene respetar al consultar material nuevo. Las secciones **01–04** cubren el lenguaje base en sí: primero la capa procedimental (variables, funciones, scope), luego las dos grandes familias de paradigmas (OOP y FP), y finalmente las estructuras de datos que ambos paradigmas comparten. Sin este núcleo sólido, todo lo demás es frágil.

Las secciones **05–06** conectan el lenguaje con el navegador: el DOM es la representación viva del documento y los eventos son el canal por el que el usuario interactúa con él. Ambas secciones son exclusivas del entorno de navegador.

Las secciones **07–08** abordan la dimensión temporal del código frontend: cómo diferir trabajo (async), cómo coordinarlo (Promesas) y cómo hablar con el exterior (fetch, WebSockets). Son el corazón de cualquier aplicación real.

Las secciones **09–10** cubren infraestructura: dónde persisten los datos en el cliente y cómo se organiza el código en unidades reutilizables. Son las que más varían entre proyectos según el toolchain elegido.

Finalmente, las secciones **11–12** son de calidad: cómo el código falla con elegancia y cómo se mantiene legible a escala. La sección **13** actúa como catálogo de referencia de las APIs de plataforma que no encajan en ningún paradigma pero que todo frontend developer necesita conocer.

## Notas relacionadas

- [[01 Programación Procedimental/index|Programación Procedimental]]
- [[02 Programación Orientada a Objetos/index|Programación Orientada a Objetos]]
- [[03 Programación Funcional/index|Programación Funcional]]
- [[04 Estructuras de Datos/index|Estructuras de Datos]]
- [[05 Manipulación del DOM/index|Manipulación del DOM]]
- [[06 Manejo de Eventos/index|Manejo de Eventos]]
- [[07 Programación Asíncrona/index|Programación Asíncrona]]
- [[08 Comunicación con APIs/index|Comunicación con APIs]]
- [[09 Almacenamiento en Cliente/index|Almacenamiento en Cliente]]
- [[10 Módulos y Organización/index|Módulos y Organización]]
- [[11 Manejo de Errores y Depuración/index|Manejo de Errores y Depuración]]
- [[12 Patrones y Buenas Prácticas/index|Patrones y Buenas Prácticas]]
- [[13 APIs del Navegador/index|APIs del Navegador]]
