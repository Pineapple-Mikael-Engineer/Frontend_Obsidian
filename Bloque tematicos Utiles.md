---
draft: true
---


# 2. BLOQUES TEMÁTICOS (MAPA CONCEPTUAL PARA OBSIDIAN)

Esta sección organiza el conocimiento por dominios conceptuales, independientemente del orden de aprendizaje. Cada bloque puede ser una carpeta en tu bóveda de Obsidian, y dentro de ella, cada concepto puede ser una nota atómica.

---

## BLOQUE 1: FUNDAMENTOS DE LA WEB Y ARQUITECTURA CLIENTE-SERVIDOR

**Explicación del dominio:** Comprender cómo funciona internet, la comunicación entre navegador y servidor, y el papel del frontend en este ecosistema.

**Temas principales:**
- Cómo funciona internet
  - Protocolo HTTP/HTTPS
  - Peticiones y respuestas
  - URLs y DNS
- Arquitectura cliente-servidor
- Rol del frontend vs backend
- Navegadores web
  - Motor de renderizado
  - Consola de desarrollador
- Estructura de un proyecto web
  - Archivos y carpetas
  - Rutas relativas y absolutas

**Subtemas específicos:**
- Métodos HTTP: GET, POST, PUT, DELETE
- Códigos de estado HTTP (200, 404, 500, etc.)
- Viewport y visualización en dispositivos

**Conceptos relacionados:** Fetch API, entorno de desarrollo, hosting

**Tecnologías involucradas:** Navegadores, servidores web

---

## BLOQUE 2: ESTRUCTURA Y SEMÁNTICA DE DOCUMENTOS (HTML)

**Explicación del dominio:** Crear la estructura de contenido de una página web con significado, no solo presentación.

**Temas principales:**
- Anatomía de un documento HTML
  - DOCTYPE
  - html, head, body
  - Metaetiquetas esenciales
- Etiquetas de contenido
  - Encabezados (h1-h6)
  - Párrafos y texto
  - Listas (ul, ol, dl)
  - Tablas (table, tr, td, th)
- Enlaces e imágenes
  - a (href, target)
  - img (src, alt)
  - map y area (imágenes con zonas cliqueables)
- Formularios
  - form (action, method)
  - input (tipos, atributos)
  - select, option, textarea, button
  - fieldset, legend, label
- HTML semántico moderno
  - header, nav, main, section, article, aside, footer
  - figure, figcaption
  - time, mark, progress, meter
- Incrustación de contenido
  - iframe
  - video y audio
  - canvas (concepto básico)

**Subtemas específicos:**
- Validación de formularios HTML5
- Atributos globales (id, class, title, lang, data-*)
- Entidades HTML (&nbsp;, &lt;, &gt;, &copy;)

**Conceptos relacionados:** DOM, SEO, accesibilidad

**Tecnologías involucradas:** HTML5

---

## BLOQUE 3: PRESENTACIÓN VISUAL (CSS)

**Explicación del dominio:** Aplicar estilos visuales a los elementos HTML.

**Temas principales:**
- Sintaxis y formas de incluir CSS
  - Selectores, propiedades, valores
  - CSS externo, interno, en línea
- Selectores CSS
  - Básicos (elemento, clase, id, universal)
  - Combinadores (descendente, hijo, adyacente, hermano)
  - Atributos ([type="text"])
  - Pseudoclases (:hover, :focus, :nth-child, etc.)
  - Pseudoelementos (::before, ::after, ::first-letter)
- Colores y fondos
  - Sistemas de color (hex, rgb, hsl)
  - background (image, repeat, position, size)
  - Gradientes lineales y radiales
- Tipografía
  - font-family, font-size, font-weight
  - @font-face y fuentes personalizadas
  - Propiedades de texto (text-align, text-decoration, etc.)
- Unidades de medida
  - Absolutas (px, pt)
  - Relativas (%, em, rem, vw, vh, vmin, vmax, ch)
- Modelo de caja (box model)
  - content, padding, border, margin
  - box-sizing
  - margin collapse
- Propiedades visuales
  - border y border-radius
  - box-shadow, text-shadow
  - opacity
- Estilos para tablas
- Estilos para formularios

**Conceptos relacionados:** Especificidad, cascada, herencia, responsive design

**Tecnologías involucradas:** CSS3

---

## BLOQUE 4: LAYOUT Y POSICIONAMIENTO

**Explicación del dominio:** Controlar la disposición y ubicación de los elementos en la página.

**Temas principales:**
- Flujo normal del documento
  - Elementos block vs inline vs inline-block
  - display (block, inline, inline-block, none)
- Posicionamiento
  - position (static, relative, absolute, fixed, sticky)
  - top, right, bottom, left
  - z-index y contexto de apilamiento
- Flexbox
  - Contenedor flex (display: flex)
  - Dirección (flex-direction)
  - Alineación (justify-content, align-items, align-content)
  - Flexibilidad (flex-grow, flex-shrink, flex-basis)
  - Orden (order)
- CSS Grid
  - Contenedor grid (display: grid)
  - Definición de columnas y filas (grid-template-columns/rows)
  - Unidad fr y función repeat()
  - Ubicación por líneas y por áreas
  - Grid implícito (grid-auto-rows/columns)
  - Alineación en grid
- Multi-columna (column-count, column-width)
- Floats (legado)
  - float (left, right)
  - clear

**Conceptos relacionados:** Responsive design, media queries

**Tecnologías involucradas:** CSS3

---

## BLOQUE 5: DISEÑO RESPONSIVO

**Explicación del dominio:** Adaptar la apariencia de un sitio a diferentes tamaños de pantalla y dispositivos.

**Temas principales:**
- Viewport meta tag
- Media queries
  - Sintaxis y operadores
  - Breakpoints comunes
  - Mobile first vs desktop first
- Unidades relativas para responsive
- Imágenes responsivas
  - max-width: 100%
  - srcset y sizes
  - elemento picture
- Tipografía fluida (clamp(), vw)
- Layouts responsivos con Flexbox y Grid
  - Flex-wrap
  - Grid auto-fit y auto-fill
- Menús responsivos (hamburguesa)

**Conceptos relacionados:** Accesibilidad, rendimiento

**Tecnologías involucradas:** CSS3, HTML5

---

## BLOQUE 6: FUNDAMENTOS DE JAVASCRIPT

**Explicación del dominio:** Conceptos básicos del lenguaje de programación JavaScript.

**Temas principales:**
- Sintaxis básica
  - Declaraciones y expresiones
  - Comentarios
- Variables
  - var, let, const
  - Hoisting
  - Ámbito (global, función, bloque)
- Tipos de datos
  - Primitivos (number, string, boolean, undefined, null, symbol)
  - Typeof
- Operadores
  - Aritméticos
  - Asignación
  - Comparación (== vs ===)
  - Lógicos (&&, ||, !)
- Conversión de tipos
  - Explícita
  - Coerción implícita
- Estructuras de control
  - if/else
  - switch
  - Operador ternario
- Bucles
  - while, do...while
  - for clásico
  - for...in (objetos)
  - for...of (iterables)
  - break, continue
- Funciones
  - Declaración y expresión
  - Parámetros y argumentos
  - Return
  - Funciones anónimas
  - Arrow functions
  - Parámetros por defecto
  - Rest parameters
  - Callbacks
- Objetos
  - Creación literal
  - Propiedades y métodos
  - Acceso por punto y corchetes
  - this en objetos
  - Object.keys, values, entries
- Arrays
  - Creación y acceso
  - Propiedades (length)
  - Métodos básicos (push, pop, shift, unshift)
  - Métodos de iteración (forEach, map, filter, reduce, find)
  - Métodos de ordenación (sort, reverse)
  - Spread operator
- Manejo de errores
  - try/catch/finally
  - throw
  - Error object

**Conceptos relacionados:** DOM, eventos, asincronía

**Tecnologías involucradas:** JavaScript (ECMAScript)

---

## BLOQUE 7: MANIPULACIÓN DEL DOM

**Explicación del dominio:** Interactuar con la estructura de la página desde JavaScript.

**Temas principales:**
- El árbol DOM
  - Nodos y elementos
  - Relaciones entre nodos
- Selección de elementos
  - getElementById
  - getElementsByClassName
  - getElementsByTagName
  - querySelector y querySelectorAll
- Recorrer el DOM
  - parentNode, parentElement
  - children, childNodes
  - firstChild, lastChild
  - nextSibling, previousSibling
  - closest
- Modificar contenido
  - textContent, innerHTML, innerText
- Modificar atributos
  - getAttribute, setAttribute
  - classList (add, remove, toggle, contains)
  - Atributos directos (id, className, src, href)
- Modificar estilos
  - style propiedad
- Crear e insertar elementos
  - createElement, createTextNode
  - appendChild, insertBefore
  - append, prepend, after, before
  - insertAdjacentHTML
- Eliminar elementos
  - removeChild, remove
- Dimensiones y posiciones
  - offsetWidth/Height, clientWidth/Height
  - getBoundingClientRect
  - scrollTop/Left, scrollIntoView

**Conceptos relacionados:** Eventos, rendimiento

**Tecnologías involucradas:** JavaScript, DOM API

---

## BLOQUE 8: EVENTOS EN JAVASCRIPT

**Explicación del dominio:** Responder a las interacciones del usuario y a otros sucesos en la página.

**Temas principales:**
- Tipos de eventos
  - Ratón (click, mouseenter, mouseleave, mousemove)
  - Teclado (keydown, keyup)
  - Formulario (submit, change, input, focus, blur)
  - Ventana (load, resize, scroll)
  - Otros (copy, cut, paste, transitionend, animationend)
- Asignación de eventos
  - Atributos HTML
  - Propiedades del elemento
  - addEventListener
- El objeto Event
  - Propiedades (type, target, currentTarget)
  - Métodos (preventDefault, stopPropagation)
- Fases del evento
  - Captura, target, bubbling
- Delegación de eventos
- Eventos personalizados
  - CustomEvent, dispatchEvent

**Conceptos relacionados:** DOM, interacción usuario

**Tecnologías involucradas:** JavaScript, Event API

---

## BLOQUE 9: ASINCRONÍA EN JAVASCRIPT

**Explicación del dominio:** Manejar operaciones que toman tiempo sin bloquear la ejecución del programa.

**Temas principales:**
- JavaScript síncrono vs asíncrono
- Callbacks
  - Patrón básico
  - Callback hell
- Promesas
  - Estados (pending, fulfilled, rejected)
  - Constructor Promise
  - then, catch, finally
  - Chaining
  - Promise.all, Promise.race, Promise.allSettled, Promise.any
- Async/await
  - Funciones async
  - await
  - try/catch con async/await
- Temporizadores
  - setTimeout, clearTimeout
  - setInterval, clearInterval
  - requestAnimationFrame
- Event Loop
  - Call stack
  - Task queue (macrotasks)
  - Microtask queue (Promesas)

**Conceptos relacionados:** Fetch API, rendimiento

**Tecnologías involucradas:** JavaScript, Event Loop

---

## BLOQUE 10: COMUNICACIÓN CON SERVIDORES (APIS)

**Explicación del dominio:** Enviar y recibir datos de servidores externos.

**Temas principales:**
- Conceptos de APIs REST
  - Endpoints
  - Métodos HTTP (GET, POST, PUT, DELETE)
  - Códigos de estado
- Fetch API
  - Sintaxis básica
  - Configuración de peticiones
  - Headers
  - Body y Content-Type
  - Manejo de respuestas
  - Manejo de errores
- XMLHttpRequest (legado)
- JSON
  - stringify y parse
  - Estructura JSON
- CORS
  - Qué es y cómo afecta
  - Soluciones básicas (proxies, etc.)
- Autenticación básica
  - API Keys
  - Tokens (concepto)

**Conceptos relacionados:** Asincronía, promesas, almacenamiento

**Tecnologías involucradas:** Fetch API, JSON

---

## BLOQUE 11: ALMACENAMIENTO EN CLIENTE

**Explicación del dominio:** Guardar datos en el navegador del usuario.

**Temas principales:**
- Cookies
  - Creación y lectura
  - Atributos (expires, path, domain, secure, HttpOnly)
- Web Storage
  - localStorage
  - sessionStorage
  - Métodos (setItem, getItem, removeItem, clear)
  - Limitaciones (solo strings, usar JSON)
- IndexedDB (concepto)
  - Cuándo usarlo
- Cache API (Service Workers, concepto básico)

**Conceptos relacionados:** JSON, persistencia

**Tecnologías involucradas:** Web Storage API, Cookie API

---

## BLOQUE 12: ACCESIBILIDAD WEB (A11Y)

**Explicación del dominio:** Hacer que los sitios web sean utilizables por personas con discapacidades.

**Temas principales:**
- HTML semántico
  - Encabezados jerárquicos
  - Landmarks (header, nav, main, etc.)
  - Listas correctas
- Atributos ARIA
  - Roles (role)
  - Propiedades (aria-label, aria-labelledby)
  - Estados (aria-expanded, aria-hidden)
- Navegación por teclado
  - tabindex
  - Gestión del foco
  - Skip links
- Contraste de color
  - WCAG ratios
- Textos alternativos
  - alt en imágenes
  - Contenido oculto para lectores de pantalla
- Formularios accesibles
  - labels asociados
  - fieldset/legend
  - Mensajes de error accesibles
- Multimedia accesible
  - Subtítulos en videos
  - Transcripciones

**Conceptos relacionados:** HTML semántico, eventos de teclado

**Tecnologías involucradas:** ARIA, HTML5

---

## BLOQUE 13: RENDIMIENTO WEB

**Explicación del dominio:** Optimizar la carga y ejecución de sitios web.

**Temas principales:**
- Optimización de imágenes
  - Formatos adecuados
  - Compresión
  - Lazy loading (loading="lazy")
- Minificación de CSS/JS
- Posición de scripts
  - async vs defer
- CSS crítico
- Reducción de reflows y repaints
  - Modificar clases vs estilos en línea
  - Transformaciones y opacidad para animaciones
- Memory leaks en JavaScript
  - Referencias no utilizadas
  - Limpieza de event listeners
- Herramientas de medición
  - Lighthouse
  - Network tab
  - Performance tab

**Conceptos relacionados:** Event Loop, animaciones

**Tecnologías involucradas:** Lighthouse, DevTools

---

## BLOQUE 14: ORGANIZACIÓN Y METODOLOGÍAS CSS

**Explicación del dominio:** Escribir CSS mantenible y escalable.

**Temas principales:**
- Especificidad
  - Cálculo
  - Conflictos
- Cascada
  - Orden de origen
- Herencia
  - Propiedades heredables
  - inherit, initial, unset
- Metodologías
  - BEM (Bloque, Elemento, Modificador)
  - OOCSS (concepto)
- Organización de archivos
  - Estructura de carpetas
- Comentarios y documentación

**Conceptos relacionados:** Selectores, buenas prácticas

**Tecnologías involucradas:** CSS

---

## BLOQUE 15: PATRONES Y BUENAS PRÁCTICAS EN JAVASCRIPT

**Explicación del dominio:** Escribir código JavaScript limpio, eficiente y mantenible.

**Temas principales:**
- Separación de responsabilidades
- Funciones puras
- Inmutabilidad
- Closures
  - Definición y usos
- Módulos
  - ES6 modules (import/export)
- Debouncing y Throttling
- Patrones de diseño básicos
  - Módulo (Module pattern)
  - Fábrica (Factory)
  - Observador (Observer) con eventos
- Principios SOLID adaptados (concepto)

**Conceptos relacionados:** Funciones, objetos, eventos

**Tecnologías involucradas:** JavaScript

---

## BLOQUE 16: HERRAMIENTAS DE DESARROLLO

**Explicación del dominio:** Utilizar herramientas para facilitar el desarrollo y depuración.

**Temas principales:**
- Navegadores
  - Chrome DevTools
  - Firefox Developer Tools
- Editores de código
  - VS Code (extensiones útiles)
- Control de versiones (concepto)
  - Git básico (init, add, commit)
- Live Server y recarga automática

**Conceptos relacionados:** Depuración, flujo de trabajo

---
