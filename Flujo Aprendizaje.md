---
draft: true
order: 3
---

# ROADMAP COMPLETO PARA DOMINAR FRONTEND BÁSICO (HTML, CSS, JavaScript)

---

# 1. FLUJO DE ESTUDIO (SÍLABO SECUENCIAL)

Este es el orden exacto que debes seguir para construir tu conocimiento desde cero, sin saltos conceptuales y asegurando que cada nuevo tema se apoya en los anteriores.

---

## FASE 0 – PREPARACIÓN Y ENTORNO DE TRABAJO

**Objetivo:** Configurar las herramientas mínimas necesarias para empezar a programar y entender cómo funciona la web por debajo.

**Conceptos fundamentales:**
- ¿Qué es un navegador web?
- ¿Qué es un servidor web?
- Diferencia entre internet y la web
- Cliente vs Servidor
- ¿Qué significa "Frontend"?

**Herramientas a instalar:**
- Navegador moderno (Chrome, Firefox, Edge)
- Editor de código (VS Code)
- Extensiones básicas: Live Server, Prettier

**Qué debo saber hacer al terminar:**
- Crear una carpeta de proyecto
- Abrirla en VS Code
- Crear archivos `.html`, `.css`, `.js`
- Ver mi página en el navegador con Live Server

---

## FASE 1 – FUNDAMENTOS DE LA WEB Y PRIMEROS PASOS CON HTML

**Objetivo:** Entender la estructura mínima de un documento web y crear contenido básico.

**Conceptos que debo estudiar:**

1. **Estructura básica de un documento HTML**
   - `<!DOCTYPE html>`
   - `<html>`
   - `<head>`
   - `<body>`
   - Codificación de caracteres (`<meta charset="UTF-8">`)

2. **Primeras etiquetas de contenido**
   - Encabezados (`<h1>` a `<h6>`)
   - Párrafos (`<p>`)
   - Saltos de línea (`<br>`)
   - Líneas horizontales (`<hr>`)

3. **Formato de texto básico**
   - `<strong>` vs `<b>`
   - `<em>` vs `<i>`
   - `<small>`, `<mark>`, `<del>`, `<ins>`

4. **Comentarios en HTML**

**Qué debo saber hacer:**
- Crear un documento HTML válido
- Escribir títulos y párrafos
- Aplicar énfasis y formato a texto

---

## FASE 2 – ENLACES, IMÁGENES Y RUTAS

**Objetivo:** Conectar documentos web y enriquecer el contenido con imágenes.

**Conceptos que debo estudiar:**

1. **Enlaces (`<a>`)**
   - Atributo `href`
   - Enlaces absolutos vs relativos
   - Enlaces internos (anclas con `id`)
   - Atributo `target="_blank"`

2. **Imágenes (`<img>`)**
   - Atributos `src` y `alt`
   - Rutas relativas y absolutas
   - Ancho y alto (atributos vs CSS)

3. **Rutas de archivos**
   - Misma carpeta (`./`)
   - Subir de nivel (`../`)
   - Desde la raíz del sitio (`/`)

**Qué debo saber hacer:**
- Crear menús de navegación simples
- Enlazar páginas entre sí
- Insertar imágenes correctamente con texto alternativo

---

## FASE 3 – LISTAS, TABLAS Y ESTRUCTURAS DE CONTENIDO

**Objetivo:** Organizar información en estructuras más complejas.

**Conceptos que debo estudiar:**

1. **Listas**
   - Listas ordenadas (`<ol>`) con tipos de numeración
   - Listas no ordenadas (`<ul>`) con viñetas
   - Listas de definición (`<dl>`, `<dt>`, `<dd>`)
   - Listas anidadas

2. **Tablas**
   - `<table>`, `<tr>`, `<td>`, `<th>`
   - Fussionar celdas (`colspan`, `rowspan`)
   - Encabezados de tabla (`<thead>`, `<tbody>`, `<tfoot>`)
   - Atributos básicos vs estilos modernos

**Qué debo saber hacer:**
- Crear menús de navegación con listas
- Presentar datos tabulares correctamente estructurados

---

## FASE 4 – HTML SEMÁNTICO Y ESTRUCTURA DE DOCUMENTO

**Objetivo:** Escribir HTML con significado, no solo presentación.

**Conceptos que debo estudiar:**

1. **Estructura semántica del documento**
   - `<header>`
   - `<nav>`
   - `<main>`
   - `<section>`
   - `<article>`
   - `<aside>`
   - `<footer>`

2. **Diferencias semánticas importantes**
   - `<section>` vs `<article>` vs `<div>`
   - Cuándo usar `<div>` (solo para agrupar sin significado)

3. **Importancia de la semántica**
   - Accesibilidad (lectores de pantalla)
   - SEO
   - Mantenibilidad del código

**Qué debo saber hacer:**
- Maquetar la estructura completa de una página web (header, navegación, contenido principal, sidebar, footer) usando etiquetas semánticas

---

## FASE 5 – FORMULARIOS HTML

**Objetivo:** Crear formularios para recoger datos del usuario.

**Conceptos que debo estudiar:**

1. **Estructura del formulario**
   - `<form>`, atributos `action` y `method`

2. **Campos de entrada**
   - `<input>` y sus tipos: `text`, `password`, `email`, `number`, `tel`, `url`, `date`, `color`, `range`, `file`, `hidden`
   - `<textarea>` (área de texto multilínea)
   - `<select>` y `<option>` (menús desplegables)
   - `<button>` (tipos: submit, reset, button)

3. **Atributos importantes**
   - `name` (clave para enviar datos)
   - `value` (valor por defecto)
   - `placeholder` (texto de ayuda)
   - `required` (campo obligatorio)
   - `disabled` y `readonly`
   - `min`, `max`, `step` (para números y rangos)
   - `pattern` (validación con expresiones regulares)

4. **Agrupación y organización**
   - `<fieldset>` y `<legend>`
   - `<label>` y su relación con `id` o anidamiento

5. **Validación de formularios**
   - Validación nativa HTML5
   - Mensajes de error personalizados

**Qué debo saber hacer:**
- Crear formularios de registro, contacto y login
- Validar campos antes de enviar (con atributos HTML)

---

## FASE 6 – INTRODUCCIÓN A CSS: SELECTORES Y PROPIEDADES BÁSICAS

**Objetivo:** Aplicar estilos por primera vez y entender cómo se conecta CSS con HTML.

**Conceptos que debo estudiar:**

1. **Formas de incluir CSS**
   - CSS en línea (atributo `style`)
   - CSS interno (`<style>` en el `<head>`)
   - CSS externo (`<link rel="stylesheet">`) → **la correcta**

2. **Sintaxis CSS**
   - Selector, propiedad, valor
   - Bloques de declaración
   - Comentarios en CSS

3. **Selectores básicos**
   - Selector de elemento (`p`, `h1`)
   - Selector de clase (`.clase`)
   - Selector de ID (`#id`)
   - Selector universal (`*`)
   - Agrupación de selectores (`,`) 

4. **Propiedades básicas de texto**
   - `color`, `background-color`
   - `font-family`, `font-size`, `font-weight`, `font-style`
   - `text-align`, `text-decoration`, `text-transform`
   - `line-height`, `letter-spacing`, `word-spacing`

**Qué debo saber hacer:**
- Cambiar colores, fuentes y alineaciones de texto
- Crear clases reutilizables

---

## FASE 7 – EL MODELO DE CAJA (BOX MODEL)

**Objetivo:** Entender cómo se dimensionan y espacian los elementos en CSS. **Este es el concepto más importante de CSS.**

**Conceptos que debo estudiar:**

1. **Partes del modelo de caja**
   - Content (contenido)
   - Padding (relleno interno)
   - Border (borde)
   - Margin (margen externo)

2. **Dimensionamiento**
   - `width`, `height`
   - `max-width`, `min-width`, `max-height`, `min-height`
   - Diferencia entre `width` y `max-width`

3. **Propiedades específicas**
   - Padding (individual y shorthand)
   - Margin (individual y shorthand, centrado con `margin: 0 auto`)
   - Border (width, style, color, shorthand)
   - `border-radius` (esquinas redondeadas)

4. **El gran dilema: `box-sizing`**
   - `content-box` (comportamiento por defecto)
   - `border-box` (el estándar moderno)
   - Por qué casi siempre usamos `border-box`

5. **Margin collapse**
   - Qué es y cómo afecta a márgenes verticales
   - Casos especiales

**Qué debo saber hacer:**
- Calcular el tamaño real de un elemento
- Controlar espacios internos y externos
- Crear tarjetas (cards) con bordes redondeados

---

## FASE 8 – FONDOS Y COLORES AVANZADOS

**Objetivo:** Dominar el uso de colores, imágenes de fondo y gradientes.

**Conceptos que debo estudiar:**

1. **Sistemas de color**
   - Nombres de colores (predefinidos)
   - Hexadecimal (#RRGGBB, #RGB)
   - RGB/RGBA (canal alfa para opacidad)
   - HSL/HSLA (tono, saturación, luminosidad)

2. **Propiedades de fondo**
   - `background-color`
   - `background-image: url()`
   - `background-repeat` (repeat, no-repeat, repeat-x, repeat-y)
   - `background-position`
   - `background-size` (cover, contain, valores)
   - `background-attachment` (scroll, fixed)
   - Shorthand `background`

3. **Gradientes**
   - Gradientes lineales (`linear-gradient()`)
   - Gradientes radiales (`radial-gradient()`)
   - Combinar múltiples fondos

4. **Sombras**
   - `box-shadow` (desplazamiento X, desplazamiento Y, desenfoque, expansión, color, inset)
   - `text-shadow`

**Qué debo saber hacer:**
- Crear fondos con imágenes y patrones
- Superponer gradientes
- Aplicar sombras realistas a cajas y textos

---

## FASE 9 – TIPOGRAFÍA AVANZADA Y FUENTES PERSONALIZADAS

**Objetivo:** Ir más allá de las fuentes del sistema y usar tipografías web.

**Conceptos que debo estudiar:**

1. **Sistemas de fuentes seguras**
   - Fuentes serif, sans-serif, monospace
   - Familias genéricas y fallbacks

2. **Fuentes personalizadas con `@font-face`**
   - Formatos: woff2, woff, ttf, eot
   - Declaración de fuentes
   - Propiedad `font-display`

3. **Google Fonts y servicios externos**
   - Cómo importar desde Google Fonts
   - Ventajas y desventajas

4. **Propiedades tipográficas avanzadas**
   - `font-variant`
   - `font-kerning`
   - `font-feature-settings`

**Qué debo saber hacer:**
- Usar cualquier tipografía en una web
- Crear una jerarquía tipográfica consistente

---

## FASE 10 – DISPLAY Y EL FLUJO DE DOCUMENTO

**Objetivo:** Entender cómo se comportan los elementos en el flujo normal y cómo cambiarlo.

**Conceptos que debo estudiar:**

1. **Tipos de elementos por defecto**
   - Elementos de bloque (`block`)
   - Elementos en línea (`inline`)
   - Elementos en línea-bloque (`inline-block`)

2. **Propiedad `display`**
   - Valores: `block`, `inline`, `inline-block`, `none`
   - Diferencias clave entre ellos

3. **Ocultar elementos**
   - `display: none` vs `visibility: hidden` vs `opacity: 0`

4. **Comportamientos especiales**
   - Elementos reemplazados (`img`, `input`, etc.)
   - Cómo se comportan en línea vs bloque

**Qué debo saber hacer:**
- Convertir elementos en línea en bloques para darles dimensiones
- Crear botones que respeten padding sin romper el flujo

---

## FASE 11 – POSICIONAMIENTO EN CSS

**Objetivo:** Sacar elementos del flujo normal y posicionarlos con precisión.

**Conceptos que debo estudiar:**

1. **La propiedad `position`**
   - `static` (por defecto)
   - `relative` (desplazamiento relativo a su posición original)
   - `absolute` (absoluto respecto al ancestro posicionado más cercano)
   - `fixed` (fijo respecto a la ventana)
   - `sticky` (híbrido entre relative y fixed)

2. **Propiedades de desplazamiento**
   - `top`, `right`, `bottom`, `left`
   - `z-index` (control de superposición y contexto de apilamiento)

3. **Contextos de apilamiento**
   - Cómo se forma un nuevo contexto

**Qué debo saber hacer:**
- Crear modales centrados
- Fijar una barra de navegación
- Hacer elementos "sticky" como encabezados de tabla
- Superponer elementos controladamente

---

## FASE 12 – FLEXBOX (DISEÑO FLEXIBLE)

**Objetivo:** Dominar el sistema de layout moderno más importante para componentes unidimensionales.

**Conceptos que debo estudiar:**

1. **Conceptos fundamentales**
   - Eje principal y eje transversal
   - Contenedor flexible (`display: flex`)
   - Items flexibles

2. **Propiedades del contenedor**
   - `flex-direction` (row, column, row-reverse, column-reverse)
   - `flex-wrap` (nowrap, wrap, wrap-reverse)
   - `justify-content` (alineación en eje principal)
   - `align-items` (alineación en eje transversal)
   - `align-content` (alineación de líneas múltiples)
   - `gap` (espacio entre items)

3. **Propiedades de los items**
   - `order` (cambiar orden visual)
   - `flex-grow` (capacidad de crecer)
   - `flex-shrink` (capacidad de encogerse)
   - `flex-basis` (tamaño base)
   - Shorthand `flex`
   - `align-self` (alineación individual)

4. **Casos de uso comunes**
   - Centrado perfecto
   - Barras de navegación
   - Tarjetas de altura igual
   - Distribución de espacio sobrante

**Qué debo saber hacer:**
- Maquetar cualquier layout unidimensional (filas o columnas)
- Crear componentes responsivos que se reordenen

---

## FASE 13 – CSS GRID (DISEÑO EN CUADRÍCULA)

**Objetivo:** Dominar el sistema de layout bidimensional más potente.

**Conceptos que debo estudiar:**

1. **Conceptos fundamentales**
   - Contenedor grid (`display: grid`)
   - Pistas (filas y columnas)
   - Celdas, áreas y líneas

2. **Definición de la cuadrícula**
   - `grid-template-columns`
   - `grid-template-rows`
   - Unidades: px, %, fr, min-content, max-content, auto
   - Funciones: `repeat()`, `minmax()`

3. **Ubicación de items**
   - Por líneas: `grid-column-start/end`, `grid-row-start/end`
   - Shorthands: `grid-column`, `grid-row`
   - Por áreas: `grid-template-areas` y `grid-area`

4. **Alineación en Grid**
   - `justify-items`, `align-items`
   - `justify-content`, `align-content`
   - `justify-self`, `align-self`

5. **Grid implícito vs explícito**
   - `grid-auto-rows`, `grid-auto-columns`
   - `grid-auto-flow`

6. **Funciones especiales**
   - `fit-content()`
   - `minmax()`
   - `repeat(auto-fill, minmax())` para layouts responsivos automáticos

**Qué debo saber hacer:**
- Crear layouts de página completos (header, footer, sidebar, contenido)
- Hacer galerías de imágenes que se adapten automáticamente
- Diseños tipo "masonry" básico

---

## FASE 14 – RESPONSIVE DESIGN Y MEDIA QUERIES

**Objetivo:** Hacer que los sitios se vean bien en todos los dispositivos.

**Conceptos que debo estudiar:**

1. **Viewport meta tag**
   - `<meta name="viewport" content="width=device-width, initial-scale=1">`
   - Por qué es obligatorio

2. **Unidades relativas**
   - Porcentajes (basados en el contenedor)
   - `em` (basado en fuente del elemento)
   - `rem` (basado en fuente raíz)
   - `vw` y `vh` (viewport width/height)
   - `vmin` y `vmax`
   - `ch` (ancho del caracter "0")

3. **Media queries**
   - Sintaxis `@media`
   - Tipos de medio: `screen`, `print`
   - Operadores lógicos: `and`, `or`, `not`
   - Características: `width`, `max-width`, `min-width`, `orientation`, `aspect-ratio`

4. **Breakpoints comunes**
   - Mobile first vs Desktop first
   - Diseño fluido (sin breakpoints fijos estrictos)

5. **Imágenes responsivas**
   - `max-width: 100%` en imágenes
   - Atributos `srcset` y `sizes` en `<img>`
   - Elemento `<picture>` para art direction

**Qué debo saber hacer:**
- Convertir cualquier diseño fijo en responsivo
- Cambiar layouts completos según el tamaño de pantalla
- Optimizar imágenes para diferentes dispositivos

---

## FASE 15 – TRANSICIONES Y ANIMACIONES CSS

**Objetivo:** Añadir movimiento y mejorar la experiencia de usuario.

**Conceptos que debo estudiar:**

1. **Transiciones (`transition`)**
   - `transition-property`
   - `transition-duration`
   - `transition-timing-function` (ease, linear, ease-in, ease-out, ease-in-out, cubic-bezier)
   - `transition-delay`
   - Shorthand `transition`

2. **Propiedades que se pueden animar**
   - Transformaciones
   - Propiedades de color, posición, tamaño

3. **Transformaciones (`transform`)**
   - `translate()`, `translateX()`, `translateY()`
   - `rotate()`
   - `scale()`
   - `skew()`
   - `matrix()` (combinación)
   - Origen de transformación (`transform-origin`)

4. **Animaciones con `@keyframes`**
   - Definición de fotogramas clave
   - `animation-name`
   - `animation-duration`
   - `animation-timing-function`
   - `animation-delay`
   - `animation-iteration-count`
   - `animation-direction`
   - `animation-fill-mode`
   - `animation-play-state`
   - Shorthand `animation`

5. **Rendimiento en animaciones**
   - Propiedades que afectan al layout vs compositor
   - Usar `transform` y `opacity` para animaciones suaves

**Qué debo saber hacer:**
- Crear hover effects atractivos
- Hover de botones con cambios suaves
- Animar la aparición de elementos
- Crear spinners y loaders

---

## FASE 16 – PSEUDOCLASES Y PSEUDOELEMENTOS

**Objetivo:** Aplicar estilos basados en estados o partes específicas de elementos.

**Conceptos que debo estudiar:**

1. **Pseudoclases de interacción**
   - `:hover` (ratón encima)
   - `:active` (momento de clic)
   - `:focus` (elemento enfocado)
   - `:focus-within` (elemento o sus hijos enfocados)
   - `:target` (elemento con ID igual al hash de URL)

2. **Pseudoclases de posición y estructura**
   - `:first-child`, `:last-child`
   - `:nth-child()` (even, odd, an+b)
   - `:nth-last-child()`
   - `:first-of-type`, `:last-of-type`
   - `:nth-of-type()`
   - `:only-child`, `:only-of-type`
   - `:empty`

3. **Pseudoclases de formulario**
   - `:checked` (checkboxes y radios seleccionados)
   - `:disabled`, `:enabled`
   - `:required`, `:optional`
   - `:valid`, `:invalid`
   - `:in-range`, `:out-of-range`

4. **Pseudoelementos**
   - `::before` y `::after` (contenido generado, propiedad `content`)
   - `::first-letter`, `::first-line`
   - `::selection` (estilo al texto seleccionado)
   - `::placeholder` (texto de placeholder en inputs)

5. **Usos creativos de pseudoelementos**
   - Decoraciones sin HTML adicional
   - Tooltips con CSS puro
   - Iconos con `content`

**Qué debo saber hacer:**
- Estilizar tablas con filas alternadas
- Crear efectos visuales en hover sin JavaScript
- Añadir decoraciones antes o después del contenido

---

## FASE 17 – ESPECIFICIDAD, CASCADA Y HERENCIA

**Objetivo:** Entender cómo CSS decide qué estilos aplicar y cómo controlarlo.

**Conceptos que debo estudiar:**

1. **La cascada**
   - Origen de los estilos (agente de usuario, autor, !important)
   - Orden de aparición (el último gana si igual especificidad)

2. **Especificidad**
   - Cálculo de especificidad: inline > ID > clase > elemento
   - Cómo calcularla manualmente
   - Herramientas del navegador para depurar

3. **Herencia**
   - Propiedades heredables (tipografía, color) vs no heredables (box model)
   - Valores especiales: `inherit`, `initial`, `unset`, `revert`

4. **El poder de `!important`**
   - Cuándo usarlo (casi nunca)
   - Cómo sobreescribirlo

5. **Resolución de conflictos**
   - Depurar por qué un estilo no se aplica

**Qué debo saber hacer:**
- Predecir qué estilo ganará en cualquier situación
- Organizar CSS para evitar guerras de especificidad
- Usar la metodología de clases específicas

---

## FASE 18 – METODOLOGÍAS CSS Y ORGANIZACIÓN DE CÓDIGO

**Objetivo:** Escribir CSS mantenible, escalable y organizado.

**Conceptos que debo estudiar:**

1. **Buenas prácticas**
   - DRY (Don't Repeat Yourself)
   - Separación de responsabilidades

2. **Metodologías**
   - **BEM** (Bloque, Elemento, Modificador): `.bloque__elemento--modificador`
   - Ventajas: especificidad plana, legibilidad
   - Cuándo usarlo

3. **Organización de archivos**
   - Estructura de carpetas
   - Archivos parciales (aunque sea CSS puro, conceptualmente)

4. **Comentarios y documentación**
   - Comentar por qué, no qué

**Qué debo saber hacer:**
- Nombrar clases de forma consistente y predecible
- Mantener un proyecto CSS de más de 1000 líneas sin volverse loco

---

## FASE 19 – INTRODUCCIÓN A JAVASCRIPT: VARIABLES, TIPOS Y OPERADORES

**Objetivo:** Dar los primeros pasos con programación en el navegador.

**Conceptos que debo estudiar:**

1. **Dónde escribir JavaScript**
   - `<script>` en HTML
   - Scripts externos (recomendado)
   - Posición en el documento (defer, async)

2. **La consola del navegador**
   - `console.log()`, `console.warn()`, `console.error()`
   - Depuración básica

3. **Variables**
   - `var` (por qué evitarla)
   - `let` (variables que cambian)
   - `const` (constantes, no reasignables)
   - Reglas de nomenclatura

4. **Tipos de datos primitivos**
   - `number` (enteros, decimales, NaN, Infinity)
   - `string` (comillas simples, dobles, backticks)
   - `boolean` (true, false)
   - `undefined`
   - `null`
   - `symbol` (conceptualmente)

5. **Operadores**
   - Aritméticos: `+`, `-`, `*`, `/`, `%`, `**`
   - Asignación: `=`, `+=`, `-=`, etc.
   - Comparación: `==`, `===`, `!=`, `!==`, `>`, `<`, `>=`, `<=`
   - Lógicos: `&&`, `||`, `!`
   - Concatenación de strings con `+`

6. **Template strings**
   - Interpolación `${variable}`
   - Strings multilínea

7. **Conversión de tipos**
   - Explícita: `Number()`, `String()`, `Boolean()`
   - Implícita (coerción)

**Qué debo saber hacer:**
- Declarar variables
- Hacer operaciones matemáticas simples
- Unir textos con variables

---

## FASE 20 – ESTRUCTURAS DE CONTROL: CONDICIONALES

**Objetivo:** Tomar decisiones en el código.

**Conceptos que debo estudiar:**

1. **Valores truthy y falsy**
   - Falsy: `false`, `0`, `""` (string vacío), `null`, `undefined`, `NaN`
   - Todo lo demás es truthy

2. **Estructura `if`, `else if`, `else`**
   - Sintaxis
   - Anidamiento (evitarlo cuando sea posible)

3. **Operador ternario**
   - `condición ? valorSiTrue : valorSiFalse`
   - Cuándo usarlo (expresiones simples)

4. **`switch`**
   - Sintaxis
   - `break` y fallthrough
   - Cuándo es mejor que múltiples if-else

**Qué debo saber hacer:**
- Validar entrada de usuario
- Cambiar comportamiento según condiciones
- Asignar valores condicionalmente

---

## FASE 21 – ESTRUCTURAS DE CONTROL: BUCLES (LOOPS)

**Objetivo:** Repetir acciones de forma controlada.

**Conceptos que debo estudiar:**

1. **Bucle `while`**
   - Sintaxis
   - Riesgo de bucle infinito

2. **Bucle `do...while`**
   - Diferencias con while (se ejecuta al menos una vez)

3. **Bucle `for` clásico**
   - Inicialización, condición, incremento
   - Usos comunes: recorrer arrays por índice

4. **`break` y `continue`**
   - Salir del bucle
   - Saltar a la siguiente iteración

5. **Bucles infinitos (y cómo evitarlos)**

**Qué debo saber hacer:**
- Repetir código un número específico de veces
- Recorrer estructuras de datos (con índices)

---

## FASE 22 – FUNCIONES EN JAVASCRIPT

**Objetivo:** Crear bloques de código reutilizables.

**Conceptos que debo estudiar:**

1. **Declaración de funciones**
   - Function declaration (con nombre)
   - Function expression (asignada a variable)

2. **Parámetros y argumentos**
   - Parámetros por defecto (ES6)
   - Rest parameters (`...args`)

3. **Return**
   - Funciones que devuelven vs que no devuelven (`void` implícito)
   - Múltiples valores con objetos/arrays

4. **Ámbito (scope)**
   - Scope global
   - Scope local (función)
   - Scope de bloque (`let` y `const`)
   - Hoisting (elevación)

5. **Funciones anónimas**
   - Usos comunes

6. **Arrow functions (funciones flecha)**
   - Sintaxis compacta
   - Return implícito (sin llaves)
   - Diferencias con funciones tradicionales (this, arguments)

7. **Funciones como ciudadanos de primera clase**
   - Asignar funciones a variables
   - Pasar funciones como argumentos (callbacks)
   - Retornar funciones desde funciones

**Qué debo saber hacer:**
- Crear funciones reutilizables para cálculos
- Separar lógica en pequeñas unidades
- Usar callbacks básicos

---

## FASE 23 – OBJETOS EN JAVASCRIPT

**Objetivo:** Estructurar datos complejos y modelar entidades del mundo real.

**Conceptos que debo estudiar:**

1. **Creación de objetos**
   - Notación literal `{}`
   - Propiedades y métodos

2. **Acceso a propiedades**
   - Notación de punto (`.`)
   - Notación de corchetes (`[]`) para propiedades dinámicas

3. **Métodos de objetos**
   - Funciones dentro de objetos

4. **Recorrer objetos**
   - `for...in`
   - `Object.keys()`, `Object.values()`, `Object.entries()`

5. **Métodos útiles de Object**
   - `Object.assign()` (copiar/mezclar objetos)
   - Spread operator (`{...obj}`) para copia superficial

6. **Referencias vs valores**
   - Los objetos se copian por referencia
   - Implicaciones al modificar objetos

7. **`this` en objetos**
   - Qué significa dentro de un método
   - Comportamiento en arrow functions (herencia léxica)

**Qué debo saber hacer:**
- Modelar datos de usuario, productos, etc.
- Actualizar propiedades de objetos
- Crear copias sin mutar el original

---

## FASE 24 – ARRAYS EN JAVASCRIPT

**Objetivo:** Manejar colecciones ordenadas de datos.

**Conceptos que debo estudiar:**

1. **Creación de arrays**
   - Notación literal `[]`
   - Constructor `new Array()`

2. **Propiedades y métodos básicos**
   - `length`
   - `push()`, `pop()` (final)
   - `unshift()`, `shift()` (inicio)

3. **Acceso y modificación**
   - Por índice
   - Modificar elementos existentes

4. **Métodos de iteración (fundamentales)**
   - `forEach()` (ejecutar función por cada elemento)
   - `map()` (transformar cada elemento y crear nuevo array)
   - `filter()` (filtrar elementos que cumplan condición)
   - `reduce()` (reducir a un solo valor)
   - `find()` (encontrar primer elemento que cumpla)
   - `findIndex()` (índice del primer elemento)
   - `some()` (al menos uno cumple)
   - `every()` (todos cumplen)

5. **Métodos de ordenación y manipulación**
   - `sort()` (ordenar, con función comparadora)
   - `reverse()` (invertir)
   - `concat()` (concatenar arrays)
   - `slice()` (extraer porción, sin modificar)
   - `splice()` (eliminar/insertar, modificando original)
   - `join()` (convertir a string)

6. **Spread operator con arrays**
   - Copiar arrays
   - Combinar arrays

7. **Arrays multidimensionales**
   - Matrices
   - Recorrer arrays anidados

**Qué debo saber hacer:**
- Procesar listas de datos
- Transformar arrays sin mutar el original
- Calcular totales, promedios
- Buscar elementos en colecciones

---

## FASE 25 – MANIPULACIÓN DEL DOM (DOCUMENT OBJECT MODEL)

**Objetivo:** Interactuar con la página web desde JavaScript para cambiar contenido y estilo.

**Conceptos que debo estudiar:**

1. **¿Qué es el DOM?**
   - Árbol de nodos
   - Representación de la página en memoria

2. **Seleccionar elementos**
   - `document.getElementById()`
   - `document.getElementsByClassName()`
   - `document.getElementsByTagName()`
   - `document.querySelector()` (selector CSS, primer elemento)
   - `document.querySelectorAll()` (todos los elementos, NodeList)

3. **Recorrer el DOM**
   - `parentNode`, `parentElement`
   - `childNodes` vs `children`
   - `firstChild`, `lastChild`
   - `nextSibling`, `previousSibling`
   - `closest()` (ancestro más cercano que cumpla selector)

4. **Modificar contenido**
   - `textContent` (solo texto)
   - `innerHTML` (HTML, peligro de seguridad)
   - `innerText` (texto con estilos aplicados)

5. **Modificar atributos**
   - `getAttribute()`, `setAttribute()`
   - Atributos directos: `id`, `className`, `src`, `href`
   - `classList` y sus métodos: `add()`, `remove()`, `toggle()`, `contains()`

6. **Modificar estilos**
   - Propiedad `style` (estilos en línea)
   - Mejor práctica: modificar clases CSS

7. **Crear e insertar elementos**
   - `document.createElement()`
   - `document.createTextNode()`
   - `appendChild()` (al final)
   - `insertBefore()` (en posición)
   - `append()` (moderno, múltiple)
   - `prepend()`, `after()`, `before()`, `replaceWith()`
   - `insertAdjacentHTML()`, `insertAdjacentElement()`

8. **Eliminar elementos**
   - `removeChild()`
   - `remove()` (directo)

9. **Dimensiones y posiciones**
   - `offsetWidth`, `offsetHeight`
   - `clientWidth`, `clientHeight`
   - `getBoundingClientRect()`
   - `scrollTop`, `scrollLeft`

**Qué debo saber hacer:**
- Actualizar contenido dinámicamente
- Añadir o quitar clases según interacción
- Crear nuevos elementos y agregarlos al DOM
- Eliminar elementos existentes

---

## FASE 26 – EVENTOS EN JAVASCRIPT

**Objetivo:** Hacer que la página responda a las acciones del usuario.

**Conceptos que debo estudiar:**

1. **Tipos de eventos**
   - Ratón: `click`, `dblclick`, `mouseenter`, `mouseleave`, `mousemove`
   - Teclado: `keydown`, `keyup`, `keypress`
   - Formulario: `submit`, `change`, `input`, `focus`, `blur`
   - Ventana: `load`, `resize`, `scroll`
   - Portapapeles: `copy`, `cut`, `paste`

2. **Formas de asignar eventos**
   - Atributos HTML (`onclick="funcion()"`) → NO RECOMENDADO
   - Propiedades del elemento (`element.onclick = funcion`)
   - `addEventListener()` (recomendado, permite múltiples)

3. **El objeto evento (`event`)**
   - Propiedades: `type`, `target`, `currentTarget`
   - Métodos: `preventDefault()` (evitar comportamiento por defecto)
   - `stopPropagation()` (evitar bubbling)
   - `stopImmediatePropagation()`

4. **Fases del evento**
   - Captura (poco usada)
   - Target
   - Bubbling (propagación hacia arriba)

5. **Delegación de eventos**
   - Añadir un listener a un padre para manejar eventos de hijos
   - Útil para elementos dinámicos

6. **Eventos personalizados**
   - `new CustomEvent()`
   - `dispatchEvent()`

**Qué debo saber hacer:**
- Responder a clics en botones
- Validar formularios en tiempo real
- Crear menús desplegables
- Implementar scroll infinito básico
- Manejar eventos en elementos creados dinámicamente

---

## FASE 27 – TEMPORIZADORES Y ASINCRONÍA BÁSICA

**Objetivo:** Ejecutar código después de un tiempo o de forma repetida.

**Conceptos que debo estudiar:**

1. **setTimeout()**
   - Ejecutar una función después de X milisegundos
   - Cancelar con `clearTimeout()`

2. **setInterval()**
   - Ejecutar una función cada X milisegundos
   - Cancelar con `clearInterval()`

3. **requestAnimationFrame()**
   - Para animaciones optimizadas
   - Sincronizado con el refresco de pantalla

4. **Callback hell**
   - Problema de anidamiento excesivo

**Qué debo saber hacer:**
- Mostrar notificaciones temporales
- Crear sliders automáticos
- Retrasar la ejecución de búsquedas (debounce básico)
- Animar con requestAnimationFrame

---

## FASE 28 – PROGRAMACIÓN ASÍNCRONA: PROMESAS

**Objetivo:** Manejar operaciones que toman tiempo (peticiones a servidores, etc.) de forma elegante.

**Conceptos que debo estudiar:**

1. **¿Qué es una promesa?**
   - Estado: pending, fulfilled, rejected
   - Representa un valor futuro

2. **Crear promesas**
   - `new Promise((resolve, reject) => {...})`

3. **Consumir promesas**
   - `.then()` (cuando se resuelve)
   - `.catch()` (cuando se rechaza)
   - `.finally()` (siempre al final)
   - Chaining de `.then()`

4. **Métodos estáticos**
   - `Promise.all()` (esperar todas, falla si una falla)
   - `Promise.allSettled()` (esperar todas, con resultado individual)
   - `Promise.race()` (la primera que se resuelva o rechace)
   - `Promise.any()` (la primera que se resuelva)

**Qué debo saber hacer:**
- Encadenar operaciones asíncronas
- Manejar errores en procesos asíncronos
- Esperar múltiples operaciones simultáneas

---

## FASE 29 – ASYNC/AWAIT

**Objetivo:** Escribir código asíncrono que parezca síncrono.

**Conceptos que debo estudiar:**

1. **Declarar funciones async**
   - Siempre devuelven una promesa

2. **La palabra clave `await`**
   - Solo dentro de funciones async
   - Pausa la ejecución hasta que la promesa se resuelva

3. **Manejo de errores con try/catch**
   - `try { await promesa } catch (error) { ... }`

4. **Async/await vs then/catch**
   - Cuándo usar cada uno

5. **Ejecución en paralelo con await**
   - `await Promise.all([...])`

**Qué debo saber hacer:**
- Refactorizar código con promesas a async/await
- Manejar errores asíncronos elegantemente

---

## FASE 30 – FETCH Y COMUNICACIÓN CON APIs

**Objetivo:** Obtener y enviar datos a servidores externos.

**Conceptos que debo estudiar:**

1. **¿Qué es una API?**
   - Concepto de petición/respuesta
   - Endpoints
   - REST

2. **Fetch API**
   - Sintaxis básica: `fetch(url)`
   - Fetch devuelve una promesa

3. **Procesar la respuesta**
   - `.json()` (para datos JSON)
   - `.text()` (para texto plano)
   - `.blob()` (para archivos binarios)

4. **Configurar la petición**
   - Segundo parámetro de fetch (objeto de opciones)
   - Método HTTP: GET, POST, PUT, DELETE
   - Headers: `Content-Type`, `Authorization`, etc.
   - Body: enviar datos con POST

5. **Manejo de errores**
   - Fetch solo rechaza por error de red, no por HTTP 404/500
   - Verificar `response.ok`

6. **Ejemplos prácticos**
   - Obtener datos de una API pública (JSONPlaceholder, etc.)
   - Enviar formularios mediante POST
   - Cargar más datos al hacer scroll

**Qué debo saber hacer:**
- Obtener datos de una API y mostrarlos en el DOM
- Enviar datos de formulario a un servidor
- Manejar estados de carga y error
- Implementar búsquedas con debounce

---

## FASE 31 – JSON (JAVASCRIPT OBJECT NOTATION)

**Objetivo:** Intercambiar datos entre cliente y servidor.

**Conceptos que debo estudiar:**

1. **Sintaxis JSON**
   - Objetos, arrays, strings, números, booleanos, null
   - Diferencias con objetos JavaScript

2. **Métodos JSON**
   - `JSON.stringify()` (convertir objeto JS a string JSON)
   - `JSON.parse()` (convertir string JSON a objeto JS)

3. **Errores comunes**
   - Propiedades con comillas dobles obligatorias
   - Sin funciones, undefined, etc.

4. **Almacenamiento local con JSON**
   - Guardar objetos en localStorage

**Qué debo saber hacer:**
- Enviar datos a una API en formato JSON
- Procesar respuestas JSON de servidores
- Persistir datos complejos en el navegador

---

## FASE 32 – ALMACENAMIENTO EN EL NAVEGADOR

**Objetivo:** Guardar datos en el lado del cliente.

**Conceptos que debo estudiar:**

1. **Cookies**
   - Para qué sirven
   - Limitaciones

2. **LocalStorage**
   - Almacenamiento persistente
   - Métodos: `setItem()`, `getItem()`, `removeItem()`, `clear()`
   - Solo guarda strings (usar JSON)

3. **SessionStorage**
   - Igual que localStorage pero por sesión (se borra al cerrar pestaña)

4. **IndexedDB (concepto)**
   - Base de datos NoSQL en el navegador
   - Cuándo es necesaria

**Qué debo saber hacer:**
- Recordar preferencias del usuario
- Implementar un carrito de compras que persista
- Guardar datos de formulario para no perderlos

---

## FASE 33 – FORMULARIOS AVANZADOS CON JAVASCRIPT

**Objetivo:** Validar y manejar formularios con JavaScript.

**Conceptos que debo estudiar:**

1. **Prevenir envío por defecto**
   - `event.preventDefault()` en submit

2. **Validación en tiempo real**
   - Evento `input` en campos
   - Mostrar mensajes de error dinámicos

3. **Obtener datos del formulario**
   - `new FormData(formElement)`
   - Convertir a objeto

4. **Validación personalizada**
   - Expresiones regulares básicas
   - Validar email, teléfono, etc.

5. **Envío asíncrono**
   - Enviar con fetch sin recargar página
   - Mostrar indicador de carga
   - Mostrar respuesta del servidor

**Qué debo saber hacer:**
- Formularios de contacto con validación en cliente y envío AJAX
- Formularios de registro con confirmación de contraseña
- Validación de campos personalizada

---

## FASE 34 – DEPURACIÓN AVANZADA

**Objetivo:** Encontrar y corregir errores de forma profesional.

**Conceptos que debo estudiar:**

1. **Herramientas del navegador**
   - Elements (inspeccionar HTML/CSS)
   - Console (mensajes, errores, advertencias)
   - Sources (depuración paso a paso, breakpoints)
   - Network (ver peticiones, respuestas, tiempos)
   - Application (almacenamiento, cookies)

2. **Breakpoints en Sources**
   - Pausar ejecución
   - Inspeccionar variables
   - Step over, step into, step out

3. **Debugger statement**
   - `debugger;` en código

4. **console avanzado**
   - `console.table()` (arrays de objetos)
   - `console.time()` y `console.timeEnd()`
   - `console.trace()`
   - `console.group()` para agrupar mensajes

**Qué debo saber hacer:**
- Encontrar errores de lógica
- Inspeccionar valores en tiempo de ejecución
- Analizar peticiones de red

---

## FASE 35 – PATRONES Y BUENAS PRÁCTICAS EN JAVASCRIPT

**Objetivo:** Escribir código limpio, mantenible y eficiente.

**Conceptos que debo estudiar:**

1. **Separación de responsabilidades**
   - HTML para estructura
   - CSS para presentación
   - JS para comportamiento

2. **Modularización**
   - Funciones que hacen una sola cosa
   - Archivos separados por funcionalidad

3. **Inmutabilidad**
   - Evitar mutar objetos/arrays originales
   - Crear copias con spread, `Object.assign()`, `Array.from()`

4. **Closures (cierres)**
   - Qué son
   - Usos prácticos: encapsulación, fábricas de funciones

5. **IIFE (Immediately Invoked Function Expression)**
   - Para crear ámbitos aislados

6. **Módulos ES6 (import/export)**
   - Exportar e importar funciones/variables
   - Módulos en el navegador (type="module")

7. **Debouncing y Throttling**
   - Optimizar eventos de alto rendimiento (scroll, resize, input)

**Qué debo saber hacer:**
- Organizar código en módulos
- Crear funciones puras sin efectos secundarios
- Optimizar la experiencia de usuario en eventos frecuentes

---

## FASE 36 – ACCESIBILIDAD WEB (A11Y)

**Objetivo:** Hacer que los sitios sean utilizables por todas las personas.

**Conceptos que debo estudiar:**

1. **HTML semántico como base**
   - Usar las etiquetas correctas
   - Jerarquía de encabezados

2. **Atributos ARIA**
   - Roles: `role="navigation"`, `role="button"`, etc.
   - Propiedades: `aria-label`, `aria-labelledby`, `aria-describedby`
   - Estados: `aria-expanded`, `aria-hidden`, `aria-checked`

3. **Navegación por teclado**
   - `tabindex` (0, -1, valores positivos)
   - Gestión del foco
   - Skip links

4. **Contraste de color**
   - WCAG ratios mínimos
   - Herramientas de verificación

5. **Textos alternativos**
   - `alt` en imágenes
   - Contenido para lectores de pantalla

6. **Formularios accesibles**
   - Asociar `<label>` correctamente
   - Agrupar con `<fieldset>`
   - Mensajes de error accesibles

**Qué debo saber hacer:**
- Auditar la accesibilidad de una página
- Hacer un menú desplegable operable con teclado
- Crear formularios comprensibles por lectores de pantalla

---

## FASE 37 – RENDIMIENTO BÁSICO

**Objetivo:** Optimizar la carga y ejecución de páginas web.

**Conceptos que debo estudiar:**

1. **Optimización de imágenes**
   - Formatos adecuados (jpg, png, webp, avif)
   - Compresión
   - Carga diferida (lazy loading) nativo: `loading="lazy"`

2. **Minificación**
   - Eliminar espacios y comentarios en CSS/JS
   - Herramientas básicas (manual o servicios)

3. **Posición de scripts**
   - Scripts al final del body
   - Atributos `async` y `defer`

4. **CSS crítico**
   - Concepto de "above the fold"

5. **Reducir reflows y repaints**
   - Modificar clases en lugar de estilos individuales
   - Usar `transform` en animaciones

**Qué debo saber hacer:**
- Identificar cuellos de botella en Lighthouse
- Implementar lazy loading básico
- Mejorar la puntuación de rendimiento

---

## FASE 38 – PROYECTO FINAL INTEGRADOR

**Objetivo:** Construir una aplicación completa que use todos los conocimientos adquiridos.

**Qué debo hacer:**
Un proyecto que incluya:

- HTML semántico y accesible
- CSS moderno con Flexbox/Grid
- Diseño completamente responsivo
- Interactividad con JavaScript
- Manipulación del DOM
- Eventos de usuario
- Fetch a una API externa
- Almacenamiento en localStorage
- Validación de formularios
- Buenas prácticas de código

**Ejemplos de proyectos:**
- Aplicación de clima (API meteorológica)
- Buscador de películas (OMDb API)
- Gestor de tareas (CRUD con localStorage)
- Carrito de compras
- Blog con carga de posts desde API

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

