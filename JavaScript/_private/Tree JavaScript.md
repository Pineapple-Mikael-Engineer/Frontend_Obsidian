---
title: Tree JavaScript
draft: true
---
# Tree

```tree
JavaScript/
│
├── 01 Programación Procedimental/
│   ├── index.md
│   ├── 01 Sintaxis Básica/
│   │   ├── index.md
│   │   ├── 01 Declaraciones y Expresiones.md
│   │   ├── 02 Punto y Coma.md
│   │   ├── 03 Comentarios.md
│   │   └── 04 Modo Estricto (use strict).md
│   ├── 02 Variables/
│   │   ├── index.md
│   │   ├── 01 var.md
│   │   ├── 02 let.md
│   │   ├── 03 const.md
│   │   ├── 04 Hoisting.md
│   │   ├── 05 Temporal Dead Zone.md
│   │   ├── 06 Ámbito (scope)/
│   │   │   ├── index.md
│   │   │   ├── 01 Global Scope.md
│   │   │   ├── 02 Function Scope.md
│   │   │   ├── 03 Block Scope.md
│   │   │   └── 04 Lexical Scope.md
│   │   └── 07 Buenas Prácticas en Nombrado.md
│   ├── 03 Tipos de Datos/
│   │   ├── index.md
│   │   ├── 01 Primitivos/
│   │   │   ├── index.md
│   │   │   ├── 01 number.md
│   │   │   ├── 02 string.md
│   │   │   ├── 03 boolean.md
│   │   │   ├── 04 undefined.md
│   │   │   ├── 05 null.md
│   │   │   ├── 06 symbol.md
│   │   │   └── 07 bigint.md
│   │   ├── 02 typeof.md
│   │   ├── 03 instanceof.md
│   │   └── 04 Conversión de Tipos/
│   │       ├── index.md
│   │       ├── 01 Conversión Explícita (Number, String, Boolean).md
│   │       ├── 02 Conversión Implícita.md
│   │       ├── 03 Truthy y Falsy.md
│   │       └── 04 Igualdad (== vs ===).md
│   ├── 04 Operadores/
│   │   ├── index.md
│   │   ├── 01 Asignación.md
│   │   ├── 02 Aritméticos.md
│   │   ├── 03 Comparación.md
│   │   ├── 04 Lógicos.md
│   │   ├── 05 Bitwise.md
│   │   ├── 06 Ternario.md
│   │   ├── 07 Nullish Coalescing (??).md
│   │   ├── 08 Optional Chaining (?.).md
│   │   └── 09 Operador in.md
│   ├── 05 Condicionales/
│   │   ├── index.md
│   │   ├── 01 if, else if, else.md
│   │   ├── 02 switch.md
│   │   └── 03 Operador Ternario.md
│   ├── 06 Bucles/
│   │   ├── index.md
│   │   ├── 01 while.md
│   │   ├── 02 do...while.md
│   │   ├── 03 for Clásico.md
│   │   ├── 04 for...in.md
│   │   ├── 05 for...of.md
│   │   ├── 06 for await...of.md
│   │   ├── 07 break y continue.md
│   │   └── 08 Labeled Statements.md
│   └── 07 Funciones (básico)/
│       ├── index.md
│       ├── 01 Declaración de Función.md
│       ├── 02 Expresión de Función.md
│       ├── 03 Parámetros por Defecto.md
│       ├── 04 Rest Parameters.md
│       ├── 05 Objeto arguments.md
│       ├── 06 Paso por Valor vs Referencia.md
│       ├── 07 Return.md
│       └── 08 Funciones de Primera Clase.md
│
├── 02 Programación Orientada a Objetos/
│   ├── index.md
│   ├── 01 Objetos Literales/
│   │   ├── index.md
│   │   ├── 01 Creación y Propiedades.md
│   │   ├── 02 Métodos.md
│   │   ├── 03 Acceso (dot y bracket).md
│   │   ├── 04 Propiedades Calculadas.md
│   │   ├── 05 Shorthand de Propiedades y Métodos.md
│   │   ├── 06 Getters y Setters.md
│   │   ├── 07 Recorrer Propiedades (keys, values, entries).md
│   │   └── 08 Comprobar y Eliminar (in, hasOwnProperty, delete).md
│   ├── 02 Prototipos/
│   │   ├── index.md
│   │   ├── 01 Cadena de Prototipos.md
│   │   ├── 02 getPrototypeOf y setPrototypeOf.md
│   │   ├── 03 Propiedades Propias vs Heredadas.md
│   │   ├── 04 Object.create().md
│   │   ├── 05 Funciones Constructoras (new, this).md
│   │   └── 06 Herencia Prototípica.md
│   ├── 03 Clases/
│   │   ├── index.md
│   │   ├── 01 Declaración de Clase.md
│   │   ├── 02 Constructor.md
│   │   ├── 03 Métodos de Instancia.md
│   │   ├── 04 Propiedades de Instancia.md
│   │   ├── 05 Métodos y Propiedades Estáticas.md
│   │   ├── 06 Campos Privados (#).md
│   │   ├── 07 Getters y Setters.md
│   │   ├── 08 Herencia (extends, super).md
│   │   └── 09 Clases como Azúcar Sintáctico.md
│   ├── 04 this/
│   │   ├── index.md
│   │   ├── 01 this Global.md
│   │   ├── 02 this en Función.md
│   │   ├── 03 this en Método.md
│   │   ├── 04 this en Constructor.md
│   │   ├── 05 this en Arrow Functions.md
│   │   ├── 06 Pérdida de this.md
│   │   ├── 07 call().md
│   │   ├── 08 apply().md
│   │   └── 09 bind().md
│   └── 05 Encapsulación y Abstracción/
│       ├── index.md
│       ├── 01 Closures para Datos Privados.md
│       ├── 02 Module Pattern (IIFE + closures).md
│       └── 03 Getters y Setters para Control de Acceso.md
│
├── 03 Programación Funcional/
│   ├── index.md
│   ├── 01 Conceptos Fundamentales/
│   │   ├── index.md
│   │   ├── 01 Funciones Puras.md
│   │   ├── 02 Inmutabilidad.md
│   │   ├── 03 Efectos Secundarios.md
│   │   ├── 04 Composición de Funciones.md
│   │   └── 05 Declarativo vs Imperativo.md
│   ├── 02 Funciones de Orden Superior/
│   │   ├── index.md
│   │   ├── 01 Recibir Funciones (callbacks).md
│   │   └── 02 Retornar Funciones.md
│   ├── 03 Arrow Functions/
│   │   ├── index.md
│   │   ├── 01 Sintaxis y Return Implícito.md
│   │   ├── 02 Sin this Propio.md
│   │   ├── 03 Sin arguments ni new.md
│   │   └── 04 Cuándo Usarlas.md
│   ├── 04 Métodos de Array Funcionales/
│   │   ├── index.md
│   │   ├── 01 forEach.md
│   │   ├── 02 map.md
│   │   ├── 03 filter.md
│   │   ├── 04 reduce.md
│   │   ├── 05 reduceRight.md
│   │   ├── 06 find y findIndex.md
│   │   ├── 07 findLast y findLastIndex.md
│   │   ├── 08 some.md
│   │   ├── 09 every.md
│   │   ├── 10 flat.md
│   │   ├── 11 flatMap.md
│   │   └── 12 Encadenamiento de Métodos.md
│   ├── 05 Inmutabilidad Práctica/
│   │   ├── index.md
│   │   ├── 01 Spread Operator.md
│   │   ├── 02 Object.assign().md
│   │   ├── 03 Array.from().md
│   │   ├── 04 concat y slice.md
│   │   └── 05 toSorted, toReversed, toSpliced.md
│   └── 06 Currying y Composición/
│       ├── index.md
│       ├── 01 Currying.md
│       ├── 02 Partial Application.md
│       ├── 03 compose.md
│       └── 04 pipe.md
│
├── 04 Estructuras de Datos/
│   ├── index.md
│   ├── 01 Arrays/
│   │   ├── index.md
│   │   ├── 01 Creación.md
│   │   ├── 02 Acceso por Índice y length.md
│   │   ├── 03 push y pop.md
│   │   ├── 04 shift y unshift.md
│   │   ├── 05 splice.md
│   │   ├── 06 slice.md
│   │   ├── 07 concat.md
│   │   ├── 08 includes, indexOf, lastIndexOf.md
│   │   ├── 09 join.md
│   │   ├── 10 sort.md
│   │   ├── 11 reverse.md
│   │   ├── 12 fill y copyWithin.md
│   │   ├── 13 Destructuring de Arrays.md
│   │   └── 14 Arrays Multidimensionales.md
│   ├── 02 Map/
│   │   ├── index.md
│   │   ├── 01 Creación y set/get.md
│   │   ├── 02 has, delete, clear, size.md
│   │   ├── 03 Iteradores (keys, values, entries).md
│   │   └── 04 Diferencias con Objetos.md
│   ├── 03 Set/
│   │   ├── index.md
│   │   ├── 01 Creación y add.md
│   │   ├── 02 has, delete, clear, size.md
│   │   └── 03 Eliminar Duplicados.md
│   ├── 04 WeakMap y WeakSet/
│   │   ├── index.md
│   │   ├── 01 Claves Débiles y Garbage Collection.md
│   │   └── 02 Casos de Uso.md
│   └── 05 Typed Arrays/
│       ├── index.md
│       ├── 01 ArrayBuffer.md
│       ├── 02 DataView.md
│       └── 03 Vistas TypedArray.md
│
├── 05 Manipulación del DOM/
│   ├── index.md
│   ├── 01 Selección de Elementos/
│   │   ├── index.md
│   │   ├── 01 getElementById.md
│   │   ├── 02 getElementsByClassName y TagName.md
│   │   ├── 03 querySelector.md
│   │   ├── 04 querySelectorAll.md
│   │   └── 05 HTMLCollection vs NodeList.md
│   ├── 02 Recorrer el DOM/
│   │   ├── index.md
│   │   ├── 01 parentNode y parentElement.md
│   │   ├── 02 childNodes y children.md
│   │   ├── 03 Hermanos (nextSibling, previousSibling).md
│   │   ├── 04 closest.md
│   │   └── 05 contains.md
│   ├── 03 Modificar Contenido/
│   │   ├── index.md
│   │   ├── 01 textContent.md
│   │   ├── 02 innerHTML (XSS).md
│   │   ├── 03 innerText.md
│   │   ├── 04 outerHTML.md
│   │   └── 05 insertAdjacentHTML.md
│   ├── 04 Modificar Atributos/
│   │   ├── index.md
│   │   ├── 01 getAttribute y setAttribute.md
│   │   ├── 02 removeAttribute y hasAttribute.md
│   │   ├── 03 Atributos Directos.md
│   │   ├── 04 classList.md
│   │   └── 05 dataset.md
│   ├── 05 Modificar Estilos/
│   │   ├── index.md
│   │   ├── 01 Propiedad style.md
│   │   └── 02 getComputedStyle.md
│   ├── 06 Crear e Insertar Elementos/
│   │   ├── index.md
│   │   ├── 01 createElement y createTextNode.md
│   │   ├── 02 appendChild y append.md
│   │   ├── 03 prepend.md
│   │   ├── 04 insertBefore.md
│   │   ├── 05 before y after.md
│   │   ├── 06 cloneNode.md
│   │   └── 07 replaceChild y replaceWith.md
│   ├── 07 Eliminar Elementos/
│   │   ├── index.md
│   │   ├── 01 remove.md
│   │   └── 02 removeChild.md
│   ├── 08 Dimensiones y Posiciones/
│   │   ├── index.md
│   │   ├── 01 offsetWidth y offsetHeight.md
│   │   ├── 02 clientWidth y clientHeight.md
│   │   ├── 03 scrollWidth y scrollHeight.md
│   │   ├── 04 getBoundingClientRect.md
│   │   └── 05 Posiciones de Scroll.md
│   └── 09 Actualizaciones Eficientes/
│       ├── index.md
│       ├── 01 DocumentFragment.md
│       ├── 02 requestAnimationFrame.md
│       └── 03 Reflows y Repaints.md
│
├── 06 Manejo de Eventos/
│   ├── index.md
│   ├── 01 Tipos de Eventos/
│   │   ├── index.md
│   │   ├── 01 Eventos de Ratón.md
│   │   ├── 02 Eventos de Teclado.md
│   │   ├── 03 Eventos de Formulario.md
│   │   ├── 04 Eventos de Ventana y Documento.md
│   │   ├── 05 Eventos de Portapapeles.md
│   │   ├── 06 Drag and Drop.md
│   │   ├── 07 Eventos de Medios.md
│   │   └── 08 Eventos de Animación y Transición CSS.md
│   ├── 02 Registro de Eventos/
│   │   ├── index.md
│   │   ├── 01 addEventListener.md
│   │   ├── 02 Opciones (once, passive, capture).md
│   │   ├── 03 removeEventListener.md
│   │   └── 04 Métodos Antiguos (onclick).md
│   ├── 03 El Objeto Event/
│   │   ├── index.md
│   │   ├── 01 target vs currentTarget.md
│   │   ├── 02 preventDefault.md
│   │   ├── 03 stopPropagation y stopImmediatePropagation.md
│   │   └── 04 Propiedades Específicas (clientX, key).md
│   ├── 04 Fases del Evento/
│   │   ├── index.md
│   │   ├── 01 Captura.md
│   │   ├── 02 Target.md
│   │   └── 03 Burbujeo.md
│   ├── 05 Delegación de Eventos.md
│   └── 06 Eventos Personalizados/
│       ├── index.md
│       ├── 01 CustomEvent y dispatchEvent.md
│       └── 02 Pasar Datos con detail.md
│
├── 07 Programación Asíncrona/
│   ├── index.md
│   ├── 01 Síncrono vs Asíncrono/
│   │   ├── index.md
│   │   ├── 01 Single-Threaded.md
│   │   └── 02 Bloqueante vs No Bloqueante.md
│   ├── 02 Event Loop/
│   │   ├── index.md
│   │   ├── 01 Call Stack.md
│   │   ├── 02 Web APIs.md
│   │   ├── 03 Task Queue (Macrotasks).md
│   │   ├── 04 Microtask Queue.md
│   │   └── 05 Orden de Ejecución (micro > macro).md
│   ├── 03 Temporizadores/
│   │   ├── index.md
│   │   ├── 01 setTimeout.md
│   │   ├── 02 setInterval.md
│   │   └── 03 requestAnimationFrame.md
│   ├── 04 Callbacks/
│   │   ├── index.md
│   │   ├── 01 Patrón Básico.md
│   │   ├── 02 Callback Hell.md
│   │   └── 03 Error-First Pattern.md
│   ├── 05 Promesas/
│   │   ├── index.md
│   │   ├── 01 Estados (pending, fulfilled, rejected).md
│   │   ├── 02 Crear Promesa (resolve, reject).md
│   │   ├── 03 then.md
│   │   ├── 04 catch.md
│   │   ├── 05 finally.md
│   │   ├── 06 Chaining y Propagación de Errores.md
│   │   ├── 07 Promise.all.md
│   │   ├── 08 Promise.allSettled.md
│   │   ├── 09 Promise.race.md
│   │   └── 10 Promise.any.md
│   ├── 06 Async / Await/
│   │   ├── index.md
│   │   ├── 01 Función async.md
│   │   ├── 02 await.md
│   │   ├── 03 Manejo de Errores (try/catch).md
│   │   ├── 04 Paralelismo con Promise.all.md
│   │   └── 05 Bucles Asíncronos (for await...of).md
│   └── 07 Tareas en Segundo Plano/
│       ├── index.md
│       ├── 01 Web Workers.md
│       └── 02 Service Workers.md
│
├── 08 Comunicación con APIs/
│   ├── index.md
│   ├── 01 Fetch API/
│   │   ├── index.md
│   │   ├── 01 Sintaxis Básica.md
│   │   ├── 02 Configuración (method, headers, body).md
│   │   ├── 03 mode, credentials, cache.md
│   │   ├── 04 Objeto Response.md
│   │   ├── 05 Procesar Respuesta (json, text, blob).md
│   │   ├── 06 Manejo de Errores.md
│   │   └── 07 AbortController.md
│   ├── 02 XMLHttpRequest (legado)/
│   │   ├── index.md
│   │   ├── 01 open y send.md
│   │   └── 02 Eventos y readyState.md
│   ├── 03 JSON/
│   │   ├── index.md
│   │   ├── 01 Sintaxis JSON.md
│   │   ├── 02 JSON.stringify (replacer, space).md
│   │   └── 03 JSON.parse (reviver).md
│   ├── 04 FormData/
│   │   ├── index.md
│   │   ├── 01 Crear desde Formulario.md
│   │   ├── 02 Métodos (append, get, set).md
│   │   └── 03 Enviar con Fetch.md
│   ├── 05 CORS/
│   │   ├── index.md
│   │   ├── 01 Same-Origin Policy.md
│   │   ├── 02 Peticiones Simples vs Preflight.md
│   │   ├── 03 Cabeceras CORS.md
│   │   └── 04 Credenciales.md
│   └── 06 WebSockets/
│       ├── index.md
│       ├── 01 Conexión Persistente.md
│       └── 02 Eventos (open, message, error, close).md
│
├── 09 Almacenamiento en Cliente/
│   ├── index.md
│   ├── 01 Cookies/
│   │   ├── index.md
│   │   ├── 01 Leer y Escribir document.cookie.md
│   │   ├── 02 Atributos (expires, path, domain).md
│   │   └── 03 HttpOnly, Secure, SameSite.md
│   ├── 02 Web Storage/
│   │   ├── index.md
│   │   ├── 01 localStorage.md
│   │   ├── 02 sessionStorage.md
│   │   └── 03 Evento storage.md
│   ├── 03 IndexedDB/
│   │   ├── index.md
│   │   ├── 01 Conceptos (stores, índices).md
│   │   └── 02 Transacciones.md
│   └── 04 Cache API.md
│
├── 10 Módulos y Organización/
│   ├── index.md
│   ├── 01 Módulos ES6/
│   │   ├── index.md
│   │   ├── 01 export (named y default).md
│   │   ├── 02 import.md
│   │   ├── 03 Módulos en el Navegador (type=module).md
│   │   ├── 04 Import Maps.md
│   │   └── 05 Import Dinámico.md
│   ├── 02 Module Pattern (IIFE)/
│   │   ├── index.md
│   │   ├── 01 IIFE.md
│   │   ├── 02 Encapsulación con Closures.md
│   │   └── 03 Patrón Revelador.md
│   └── 03 Bundling (concepto).md
│
├── 11 Manejo de Errores y Depuración/
│   ├── index.md
│   ├── 01 Tipos de Errores/
│   │   ├── index.md
│   │   ├── 01 SyntaxError.md
│   │   ├── 02 ReferenceError.md
│   │   ├── 03 TypeError.md
│   │   ├── 04 RangeError.md
│   │   └── 05 Otros (URIError, EvalError).md
│   ├── 02 try / catch / finally/
│   │   ├── index.md
│   │   ├── 01 try y catch.md
│   │   └── 02 finally.md
│   ├── 03 throw/
│   │   ├── index.md
│   │   ├── 01 Lanzar Errores.md
│   │   └── 02 Errores Personalizados (extends Error).md
│   └── 04 Depuración/
│       ├── index.md
│       ├── 01 Métodos de console.md
│       ├── 02 debugger y Breakpoints.md
│       └── 03 Step y Watch Expressions.md
│
├── 12 Patrones y Buenas Prácticas/
│   ├── index.md
│   ├── 01 Debouncing y Throttling/
│   │   ├── index.md
│   │   ├── 01 Debounce.md
│   │   └── 02 Throttle.md
│   ├── 02 Closures Avanzados/
│   │   ├── index.md
│   │   ├── 01 Fábricas de Funciones.md
│   │   └── 02 Datos Privados.md
│   ├── 03 IIFE.md
│   ├── 04 Memoización.md
│   ├── 05 Patrones de Diseño/
│   │   ├── index.md
│   │   ├── 01 Singleton.md
│   │   └── 02 Observer (Pub/Sub).md
│   ├── 06 Separación de Responsabilidades.md
│   └── 07 Código Limpio.md
│
└── 13 APIs del Navegador/
    ├── index.md
    ├── 01 Window/
    │   ├── index.md
    │   ├── 01 Dimensiones y Scroll.md
    │   ├── 02 open y close.md
    │   └── 03 alert, confirm, prompt.md
    ├── 02 Navigator/
    │   ├── index.md
    │   ├── 01 userAgent y language.md
    │   ├── 02 onLine.md
    │   ├── 03 geolocation.md
    │   ├── 04 mediaDevices.md
    │   ├── 05 clipboard.md
    │   └── 06 share.md
    ├── 03 Location y URL/
    │   ├── index.md
    │   ├── 01 Propiedades de location.md
    │   ├── 02 reload, assign, replace.md
    │   └── 03 URLSearchParams.md
    ├── 04 History/
    │   ├── index.md
    │   ├── 01 back, forward, go.md
    │   ├── 02 pushState y replaceState.md
    │   └── 03 Evento popstate.md
    ├── 05 Canvas API/
    │   ├── index.md
    │   ├── 01 Contexto 2D.md
    │   ├── 02 Dibujar Formas y Trazados.md
    │   ├── 03 Estilos (fillStyle, strokeStyle).md
    │   ├── 04 Texto e Imágenes.md
    │   └── 05 Transformaciones.md
    ├── 06 Observers/
    │   ├── index.md
    │   ├── 01 Intersection Observer.md
    │   ├── 02 Mutation Observer.md
    │   └── 03 Resize Observer.md
    ├── 07 Performance API.md
    └── 08 Web Components/
        ├── index.md
        ├── 01 Custom Elements.md
        ├── 02 Shadow DOM.md
        └── 03 HTML Templates.md
```

**Fuente:** destilado de `ARQUITECTURA.md` (atómico máximo, estilo Python: numerado por nivel, una nota por concepto/método).
