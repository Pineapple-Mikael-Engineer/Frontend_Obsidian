---
draft: true
---

# ARQUITECTURA DE BÓVEDA OBSIDIAN PARA FRONTEND FUNDAMENTAL

Este documento establece la estructura conceptual jerárquica para organizar todo el conocimiento de HTML, CSS y JavaScript en una bóveda de Obsidian. Cada nodo terminal representa un concepto que puede convertirse en una nota independiente.

---

# HTML (LENGUAJE DE MARCADO DE HIPERTEXTO)

## Estructura de Documento
```
HTML
├── Estructura de Documento
│   ├── Declaración DOCTYPE
│   ├── Elemento raíz (html)
│   ├── Cabecera del documento (head)
│   │   ├── Codificación de caracteres (meta charset)
│   │   ├── Viewport (meta viewport)
│   │   ├── Título del documento (title)
│   │   ├── Descripción (meta description)
│   │   ├── Palabras clave (meta keywords)
│   │   ├── Autor (meta author)
│   │   ├── Enlace a CSS (link)
│   │   ├── Estilos internos (style)
│   │   ├── Scripts (script)
│   │   ├── Favicon (link rel="icon")
│   │   ├── Open Graph (meta property="og:*")
│   │   └── Twitter Cards (meta name="twitter:*")
│   └── Cuerpo del documento (body)
│
├── Estructura Semántica
│   ├── Encabezados jerárquicos (h1-h6)
│   ├── Navegación principal (nav)
│   ├── Contenido principal (main)
│   ├── Secciones genéricas (section)
│   ├── Artículos independientes (article)
│   ├── Contenido complementario (aside)
│   ├── Cabecera de sección (header)
│   ├── Pie de sección (footer)
│   ├── Dirección/contacto (address)
│   ├── Título de sección agrupada (hgroup)
│   └── Figura con leyenda (figure, figcaption)
│
├── Texto y Contenido
│   ├── Párrafos (p)
│   ├── Saltos de línea (br)
│   ├── Línea horizontal (hr)
│   ├── Énfasis fuerte (strong)
│   ├── Énfasis (em)
│   ├── Texto en negrita sin énfasis (b)
│   ├── Texto en cursiva sin énfasis (i)
│   ├── Texto pequeño (small)
│   ├── Texto subrayado (u)
│   ├── Texto tachado (s, del)
│   ├── Texto insertado (ins)
│   ├── Texto resaltado (mark)
│   ├── Citas en bloque (blockquote)
│   ├── Citas en línea (q)
│   ├── Atributo cite
│   ├── Abreviaturas (abbr)
│   ├── Definiciones (dfn)
│   ├── Código (code)
│   ├── Código preformateado (pre)
│   ├── Variable (var)
│   ├── Salida de computadora (samp)
│   ├── Entrada de teclado (kbd)
│   ├── Superíndice (sup)
│   ├── Subíndice (sub)
│   ├── Tiempo (time)
│   ├── Fecha/hora (datetime)
│   ├── Texto bidireccional (bdo, bdi)
│   ├── Ruptura de palabra (wbr)
│   └── Contenido preformateado con sintaxis (pre + code)
│
├── Listas
│   ├── Listas ordenadas (ol)
│   │   ├── Atributo type (1, A, a, I, i)
│   │   ├── Atributo start
│   │   ├── Atributo reversed
│   │   └── Elementos de lista (li)
│   ├── Listas no ordenadas (ul)
│   │   └── Elementos de lista (li)
│   ├── Listas de definición (dl)
│   │   ├── Término (dt)
│   │   └── Descripción (dd)
│   ├── Listas anidadas
│   └── Listas con iconos personalizados
│
├── Enlaces y Navegación
│   ├── Enlaces básicos (a)
│   │   ├── Atributo href
│   │   ├── Atributo target (_self, _blank, _parent, _top)
│   │   ├── Atributo rel (nofollow, noopener, noreferrer)
│   │   ├── Atributo download
│   │   ├── Atributo hreflang
│   │   └── Atributo type
│   ├── Anclas internas (#id)
│   ├── Enlaces a correo (mailto:)
│   ├── Enlaces telefónicos (tel:)
│   ├── Enlaces a protocolos personalizados
│   └── Navegación con teclado (accesskey)
│
├── Tablas
│   ├── Contenedor de tabla (table)
│   ├── Fila de tabla (tr)
│   ├── Celda de encabezado (th)
│   │   ├── Atributo scope (col, row, colgroup, rowgroup)
│   │   └── Atributo abbr
│   ├── Celda de datos (td)
│   ├── Fusión de columnas (colspan)
│   ├── Fusión de filas (rowspan)
│   ├── Agrupación de columnas (colgroup, col)
│   ├── Encabezado de tabla (thead)
│   ├── Cuerpo de tabla (tbody)
│   ├── Pie de tabla (tfoot)
│   ├── Título de tabla (caption)
│   └── Tablas complejas (múltiples encabezados)
│
├── Formularios
│   ├── Contenedor de formulario (form)
│   │   ├── Atributo action
│   │   ├── Atributo method (GET, POST)
│   │   ├── Atributo enctype (application/x-www-form-urlencoded, multipart/form-data, text/plain)
│   │   ├── Atributo target
│   │   ├── Atributo novalidate
│   │   ├── Atributo autocomplete
│   │   └── Atributo name
│   │
│   ├── Campos de entrada (input)
│   │   ├── Atributos comunes (type, name, value, placeholder, required, disabled, readonly, autofocus, autocomplete, tabindex)
│   │   ├── Tipos de input (text, password, email, number, tel, url, search, hidden, checkbox, radio, file, date, datetime-local, month, week, time, color, range, image, submit, reset, button)
│   │   ├── Atributos específicos (min, max, step, multiple, accept, checked, pattern, maxlength, minlength, size, list (datalist), alt (para image), src (para image), height/width (para image))
│   │   └── Validación específica por tipo
│   │
│   ├── Área de texto (textarea)
│   │   ├── Atributo rows
│   │   ├── Atributo cols
│   │   ├── Atributo wrap
│   │   └── Atributo maxlength
│   │
│   ├── Listas de selección (select)
│   │   ├── Opciones (option)
│   │   │   ├── Atributo value
│   │   │   ├── Atributo selected
│   │   │   └── Atributo disabled
│   │   ├── Agrupación de opciones (optgroup)
│   │   │   └── Atributo label
│   │   ├── Atributo multiple (para selección múltiple)
│   │   └── Atributo size
│   │
│   ├── Lista de datos (datalist)
│   │   └── Opciones predefinidas para autocompletado
│   │
│   ├── Botones (button)
│   │   ├── Tipo submit (envía formulario)
│   │   ├── Tipo reset (reinicia formulario)
│   │   ├── Tipo button (sin comportamiento por defecto)
│   │   └── Atributo value
│   │
│   ├── Etiquetas y agrupación
│   │   ├── Etiqueta para campo (label)
│   │   │   ├── Asociación por for (con id)
│   │   │   └── Anidamiento del campo
│   │   ├── Agrupación de campos (fieldset)
│   │   ├── Leyenda de grupo (legend)
│   │   └── Agrupación visual
│   │
│   ├── Elementos de salida
│   │   ├── Resultado de cálculo (output)
│   │   │   ├── Atributo for (relación con campos)
│   │   │   └── Atributo name
│   │   ├── Medidor (meter)
│   │   │   ├── Atributo value
│   │   │   ├── Atributo min/max
│   │   │   ├── Atributo low/high
│   │   │   └── Atributo optimum
│   │   └── Barra de progreso (progress)
│   │       ├── Atributo value
│   │       └── Atributo max
│   │
│   └── Validación de formularios
│       ├── Validación nativa HTML5
│       ├── Atributo required
│       ├── Atributo pattern (expresiones regulares)
│       ├── Atributo min/max/step
│       ├── Atributo maxlength/minlength
│       ├── Pseudoclases de validación (:valid, :invalid, :required, :optional, :in-range, :out-of-range)
│       ├── Mensajes de error personalizados (setCustomValidity)
│       ├── API de validación de restricciones (constraint validation API)
│       └── Desactivar validación (novalidate)
│
├── Multimedia e Incrustación
│   ├── Imágenes
│   │   ├── Imagen básica (img)
│   │   │   ├── Atributo src
│   │   │   ├── Atributo alt
│   │   │   ├── Atributo width/height
│   │   │   ├── Atributo loading (lazy, eager)
│   │   │   ├── Atributo decoding (async, sync, auto)
│   │   │   ├── Atributo srcset (para imágenes responsivas)
│   │   │   ├── Atributo sizes (para imágenes responsivas)
│   │   │   └── Atributo crossorigin
│   │   ├── Mapa de imagen (map, area)
│   │   │   ├── Formas (shape: rect, circle, poly, default)
│   │   │   ├── Coordenadas (coords)
│   │   │   ├── Enlace (href)
│   │   │   └── Atributo alt
│   │   ├── Imágenes con múltiples fuentes (picture)
│   │   │   ├── Elemento source
│   │   │   │   ├── Atributo srcset
│   │   │   │   ├── Atributo media (media query)
│   │   │   │   ├── Atributo type (image/webp, etc.)
│   │   │   │   └── Atributo sizes
│   │   │   └── Fallback con img
│   │   └── Formatos de imagen (jpg, png, gif, svg, webp, avif)
│   │
│   ├── Video y Audio
│   │   ├── Video (video)
│   │   │   ├── Fuentes (source)
│   │   │   ├── Atributo src
│   │   │   ├── Atributo controls
│   │   │   ├── Atributo autoplay
│   │   │   ├── Atributo muted
│   │   │   ├── Atributo loop
│   │   │   ├── Atributo poster (imagen de portada)
│   │   │   ├── Atributo preload (none, metadata, auto)
│   │   │   ├── Atributo width/height
│   │   │   ├── Atributo playsinline (para móviles)
│   │   │   └── Pistas de texto (track)
│   │   │       ├── Atributo kind (subtitles, captions, descriptions, chapters, metadata)
│   │   │       ├── Atributo src
│   │   │       ├── Atributo srclang
│   │   │       ├── Atributo label
│   │   │       └── Atributo default
│   │   ├── Audio (audio)
│   │   │   ├── Fuentes (source)
│   │   │   ├── Atributo src
│   │   │   ├── Atributo controls
│   │   │   ├── Atributo autoplay
│   │   │   ├── Atributo muted
│   │   │   ├── Atributo loop
│   │   │   ├── Atributo preload
│   │   │   └── Atributo crossorigin
│   │   └── API de medios (Media Source Extensions, conceptos básicos)
│   │
│   ├── Incrustación
│   │   ├── Marco en línea (iframe)
│   │   │   ├── Atributo src
│   │   │   ├── Atributo srcdoc
│   │   │   ├── Atributo name
│   │   │   ├── Atributo sandbox
│   │   │   ├── Atributo allow
│   │   │   ├── Atributo allowfullscreen
│   │   │   ├── Atributo loading
│   │   │   ├── Atributo referrerpolicy
│   │   │   └── Atributo width/height
│   │   ├── Incrustación genérica (embed)
│   │   ├── Objeto incrustado (object)
│   │   │   ├── Atributo data
│   │   │   ├── Atributo type
│   │   │   ├── Atributo name
│   │   │   ├── Atributo width/height
│   │   │   └── Parámetros (param)
│   │   └── Contenido canónico (link rel="canonical")
│   │
│   └── Gráficos
│       ├── Lienzo (canvas)
│       │   ├── Atributo width/height
│       │   ├── Contexto 2D (getContext("2d"))
│       │   └── Contexto WebGL (getContext("webgl"))
│       └── Gráficos vectoriales (svg)
│           ├── Elementos SVG básicos (circle, rect, line, path, text, g)
│           ├── Atributos SVG (fill, stroke, stroke-width, transform)
│           └── SVG en línea vs como imagen
│
├── Metadatos y SEO
│   ├── Metadatos estándar
│   │   ├── Título (title)
│   │   ├── Descripción (meta name="description")
│   │   ├── Palabras clave (meta name="keywords")
│   │   ├── Autor (meta name="author")
│   │   ├── Idioma (html lang)
│   │   ├── Robots (meta name="robots")
│   │   ├── Googlebot (meta name="googlebot")
│   │   ├── Verificación de sitio (meta name="google-site-verification")
│   │   └── Refresh/redirección (meta http-equiv="refresh")
│   │
│   ├── Open Graph (Facebook/LinkedIn)
│   │   ├── Título (og:title)
│   │   ├── Tipo (og:type)
│   │   ├── URL (og:url)
│   │   ├── Imagen (og:image)
│   │   ├── Descripción (og:description)
│   │   ├── Nombre del sitio (og:site_name)
│   │   ├── Locale (og:locale)
│   │   └── Imagen: ancho/alto (og:image:width, og:image:height)
│   │
│   ├── Twitter Cards
│   │   ├── Tipo de tarjeta (twitter:card)
│   │   ├── Sitio (twitter:site)
│   │   ├── Título (twitter:title)
│   │   ├── Descripción (twitter:description)
│   │   ├── Imagen (twitter:image)
│   │   ├── Creador (twitter:creator)
│   │   └── Alt de imagen (twitter:image:alt)
│   │
│   └── JSON-LD (Datos estructurados)
│       ├── Schema.org básico
│       ├── Article
│       ├── Person/Organization
│       ├── Product
│       ├── Event
│       ├── Recipe
│       ├── FAQPage
│       ├── BreadcrumbList
│       └── LocalBusiness
│
├── Accesibilidad (A11Y)
│   ├── HTML Semántico como base
│   ├── Atributos ARIA
│   │   ├── Roles ARIA (role)
│   │   │   ├── Roles de landmark (banner, navigation, main, complementary, contentinfo)
│   │   │   ├── Roles de estructura (article, section, list, listitem)
│   │   │   ├── Roles de widget (button, checkbox, tab, tablist, tabpanel)
│   │   │   ├── Roles de documento (document, img)
│   │   │   └── Roles abstractos (no usar)
│   │   ├── Propiedades ARIA (aria-*)
│   │   │   ├── aria-label (etiqueta directa)
│   │   │   ├── aria-labelledby (referencia a ID)
│   │   │   ├── aria-describedby (descripción)
│   │   │   ├── aria-details (detalles adicionales)
│   │   │   └── aria-atomic (para regiones vivas)
│   │   └── Estados ARIA (aria-* dinámicos)
│   │       ├── aria-expanded
│   │       ├── aria-hidden
│   │       ├── aria-checked
│   │       ├── aria-selected
│   │       ├── aria-pressed
│   │       ├── aria-disabled
│   │       ├── aria-readonly
│   │       ├── aria-required
│   │       ├── aria-invalid
│   │       ├── aria-busy
│   │       ├── aria-live (off, polite, assertive)
│   │       ├── aria-relevant (additions, removals, text, all)
│   │       └── aria-valuemin, aria-valuemax, aria-valuenow, aria-valuetext
│   │
│   ├── Navegación por teclado
│   │   ├── tabindex (0, -1, valores positivos)
│   │   ├── Gestión de foco (focus, blur)
│   │   ├── Orden de tabulación lógico
│   │   ├── Skip links (enlaces para saltar)
│   │   └── Trampas de foco (focus trapping)
│   │
│   ├── Textos alternativos
│   │   ├── alt en imágenes
│   │   ├── Texto para iconos
│   │   ├── Contenido oculto visualmente pero disponible para lectores (sr-only)
│   │   └── Descripciones largas (longdesc, obsoleto, alternativas)
│   │
│   ├── Formularios accesibles
│   │   ├── Asociación correcta label/input
│   │   ├── Agrupación con fieldset/legend
│   │   ├── Mensajes de error accesibles
│   │   ├── Validación accesible
│   │   └── Notificaciones de éxito/error
│   │
│   └── Multimedia accesible
│       ├── Subtítulos (captions)
│       ├── Transcripciones
│       ├── Audiodescripción
│       └── Controles accesibles
│
└── Atributos Globales
    ├── class (clases CSS)
    ├── id (identificador único)
    ├── style (estilos en línea)
    ├── title (información emergente)
    ├── lang (idioma del elemento)
    ├── dir (dirección del texto: ltr, rtl, auto)
    ├── hidden (ocultar elemento)
    ├── tabindex (orden de tabulación)
    ├── accesskey (tecla de acceso rápido)
    ├── draggable (arrastrable)
    ├── contenteditable (editable por usuario)
    ├── spellcheck (corrección ortográfica)
    ├── translate (traducible o no)
    ├── data-* (atributos personalizados)
    ├── role (rol ARIA)
    ├── aria-* (atributos ARIA)
    ├── slot (para Web Components)
    ├── part (para CSS Shadow Parts)
    ├── exportparts (para Shadow Parts)
    └── inert (inactivo)

```

---

# CSS (HOJAS DE ESTILO EN CASCADA)

## Fundamentos del Estilo
```
CSS
├── Fundamentos del Estilo
│   ├── Sintaxis CSS
│   │   ├── Selector
│   │   ├── Propiedad
│   │   ├── Valor
│   │   ├── Declaración (propiedad: valor;)
│   │   ├── Bloque de declaración { }
│   │   └── Regla completa (selector { propiedades })
│   │
│   ├── Formas de incluir CSS
│   │   ├── CSS externo (link rel="stylesheet")
│   │   ├── CSS interno (style en head)
│   │   ├── CSS en línea (style attribute)
│   │   └── @import (para importar otros CSS)
│   │
│   ├── Comentarios CSS (/* */)
│   │
│   ├── Selectores
│   │   ├── Selectores básicos
│   │   │   ├── Selector de tipo (elemento)
│   │   │   ├── Selector de clase (.clase)
│   │   │   ├── Selector de ID (#id)
│   │   │   ├── Selector universal (*)
│   │   │   └── Agrupación de selectores (selector1, selector2)
│   │   │
│   │   ├── Combinadores
│   │   │   ├── Descendente (espacio) (ancestro descendiente)
│   │   │   ├── Hijo directo (>) (padre > hijo)
│   │   │   ├── Hermano adyacente (+) (elemento + siguiente)
│   │   │   └── Hermano general (~) (elemento ~ todos los siguientes)
│   │   │
│   │   ├── Selectores de atributo
│   │   │   ├── [atributo] (tiene el atributo)
│   │   │   ├── [atributo="valor"] (igual exacto)
│   │   │   ├── [atributo^="valor"] (empieza con)
│   │   │   ├── [atributo$="valor"] (termina con)
│   │   │   ├── [atributo*="valor"] (contiene)
│   │   │   ├── [atributo~="valor"] (contiene palabra separada por espacios)
│   │   │   └── [atributo|="valor"] (empieza con valor seguido de guión)
│   │   │
│   │   └── Pseudoclases y pseudoelementos (ver sección específica)
│   │
│   ├── Colores
│   │   ├── Palabras clave de color (red, blue, transparent, currentColor)
│   │   ├── Hexadecimal (#RRGGBB, #RGB, #RRGGBBAA)
│   │   ├── RGB/RGBA (rgb(255,0,0), rgba(255,0,0,0.5))
│   │   ├── HSL/HSLA (hsl(0,100%,50%), hsla(0,100%,50%,0.5))
│   │   ├── HWB (hwb(0,0%,0%))
│   │   ├── LAB/LCH (para gamas amplias)
│   │   └── color-mix() (mezcla de colores)
│   │
│   ├── Unidades de medida
│   │   ├── Absolutas
│   │   │   ├── px (píxeles)
│   │   │   ├── pt (puntos, 1/72 pulgada)
│   │   │   ├── pc (picas, 12 puntos)
│   │   │   ├── in (pulgadas)
│   │   │   ├── cm (centímetros)
│   │   │   └── mm (milímetros)
│   │   │
│   │   ├── Relativas a fuente
│   │   │   ├── em (relativo al tamaño de fuente del elemento)
│   │   │   ├── rem (relativo al tamaño de fuente del elemento raíz)
│   │   │   ├── ex (altura de la letra "x")
│   │   │   ├── ch (ancho del caracter "0")
│   │   │   ├── lh (altura de línea del elemento)
│   │   │   └── rlh (altura de línea del elemento raíz)
│   │   │
│   │   ├── Relativas al viewport
│   │   │   ├── vw (1% del ancho del viewport)
│   │   │   ├── vh (1% del alto del viewport)
│   │   │   ├── vmin (1% del lado más pequeño)
│   │   │   ├── vmax (1% del lado más grande)
│   │   │   ├── svw/svh (small viewport)
│   │   │   ├── lvw/lvh (large viewport)
│   │   │   └── dvw/dvh (dynamic viewport)
│   │   │
│   │   ├── Porcentajes (%) (relativo al contenedor)
│   │   └── Funciones de cálculo
│   │       ├── calc() (operaciones matemáticas)
│   │       ├── min() (valor mínimo)
│   │       ├── max() (valor máximo)
│   │       └── clamp() (valor dentro de un rango)
│   │
│   ├── Propiedades de texto básicas
│   │   ├── color (color del texto)
│   │   ├── font-family (tipo de fuente)
│   │   ├── font-size (tamaño)
│   │   ├── font-weight (grosor: normal, bold, 100-900)
│   │   ├── font-style (estilo: normal, italic, oblique)
│   │   ├── font-variant (variantes: small-caps)
│   │   ├── line-height (altura de línea)
│   │   ├── text-align (alineación: left, right, center, justify)
│   │   ├── text-decoration (decoración: underline, overline, line-through)
│   │   │   ├── text-decoration-line
│   │   │   ├── text-decoration-color
│   │   │   ├── text-decoration-style (solid, double, dotted, dashed, wavy)
│   │   │   └── text-decoration-thickness
│   │   ├── text-transform (transformación: uppercase, lowercase, capitalize)
│   │   ├── text-indent (sangría)
│   │   ├── letter-spacing (espaciado entre letras)
│   │   ├── word-spacing (espaciado entre palabras)
│   │   └── white-space (manejo de espacios en blanco)
│   │
│   └── Herencia y cascada
│       ├── Herencia natural (propiedades que se heredan)
│       ├── Valores especiales
│       │   ├── inherit (forzar herencia)
│       │   ├── initial (valor inicial según especificación)
│       │   ├── unset (hereda si puede, sino initial)
│       │   └── revert (valor por defecto del navegador)
│       ├── Cascada (origen de estilos)
│       ├── Especificidad (cálculo)
│       └── !important (sobreescribe todo, evitar)
│
├── Modelo de Caja (Box Model)
│   ├── Partes del modelo de caja
│   │   ├── Contenido (content)
│   │   ├── Relleno (padding)
│   │   ├── Borde (border)
│   │   └── Margen (margin)
│   │
│   ├── Dimensiones
│   │   ├── width / height
│   │   ├── min-width / min-height
│   │   ├── max-width / max-height
│   │   ├── aspect-ratio (relación de aspecto)
│   │   └── fit-content, min-content, max-content
│   │
│   ├── Padding
│   │   ├── padding-top, padding-right, padding-bottom, padding-left
│   │   └── shorthand padding (1-4 valores)
│   │
│   ├── Border
│   │   ├── border-width (thin, medium, thick, valores)
│   │   ├── border-style (none, hidden, dotted, dashed, solid, double, groove, ridge, inset, outset)
│   │   ├── border-color
│   │   ├── shorthand border (width style color)
│   │   ├── border-top, border-right, border-bottom, border-left
│   │   ├── border-radius (esquinas redondeadas)
│   │   │   ├── valores individuales (border-top-left-radius, etc.)
│   │   │   └── shorthand border-radius (1-4 valores con / para elíptico)
│   │   └── border-image (imagen como borde)
│   │       ├── border-image-source
│   │       ├── border-image-slice
│   │       ├── border-image-width
│   │       ├── border-image-outset
│   │       └── border-image-repeat
│   │
│   ├── Margin
│   │   ├── margin-top, margin-right, margin-bottom, margin-left
│   │   ├── shorthand margin (1-4 valores)
│   │   └── margin: auto (centrado horizontal)
│   │
│   ├── box-sizing
│   │   ├── content-box (width incluye solo contenido)
│   │   └── border-box (width incluye contenido, padding y border)
│   │
│   └── Margin collapse
│       ├── Colapso entre elementos adyacentes
│       ├── Colapso entre padre y primer/último hijo
│       ├── Colapso de márgenes negativos
│       └── Cómo prevenirlo (padding, border, overflow)
│
├── Tipografía Avanzada
│   ├── Fuentes web (@font-face)
│   │   ├── src (url, local)
│   │   ├── font-family (nombre de la fuente)
│   │   ├── font-weight (grosor asociado)
│   │   ├── font-style (estilo asociado)
│   │   ├── font-display (auto, block, swap, fallback, optional)
│   │   ├── unicode-range (rangos de caracteres)
│   │   └── format (woff, woff2, truetype, opentype, embedded-opentype, svg)
│   │
│   ├── Propiedades tipográficas avanzadas
│   │   ├── font-kerning (ajuste de kerning)
│   │   ├── font-optical-sizing (ajuste óptico)
│   │   ├── font-variant alternativas (small-caps, oldstyle-nums, etc.)
│   │   ├── font-feature-settings (características OpenType)
│   │   ├── font-variation-settings (fuentes variables)
│   │   ├── text-rendering (optimización de renderizado)
│   │   ├── font-synthesis (síntesis de estilos no disponibles)
│   │   └── font-stretch (ancho de fuente)
│   │
│   ├── Alineación vertical
│   │   ├── vertical-align (baseline, sub, super, top, middle, bottom, etc.)
│   │   └── Alineación en celdas de tabla
│   │
│   ├── Decoración de texto avanzada
│   │   ├── text-emphasis (énfasis en texto)
│   │   ├── text-shadow (sombra)
│   │   ├── text-stroke (contorno, no estándar pero soportado)
│   │   └── text-underline-offset / text-underline-position
│   │
│   ├── Manejo de texto multilínea
│   │   ├── overflow-wrap / word-wrap (control de desbordamiento)
│   │   ├── word-break (ruptura de palabras)
│   │   ├── hyphens (guiones automáticos)
│   │   ├── line-clamp (limitar líneas)
│   │   └── text-overflow (elipsis: clip, ellipsis)
│   │
│   └── Escritura y dirección
│       ├── writing-mode (horizontal-tb, vertical-rl, vertical-lr)
│       ├── direction (ltr, rtl)
│       ├── text-orientation (en vertical)
│       └── unicode-bidi (control de bidireccionalidad)
│
├── Fondos y Efectos Visuales
│   ├── Propiedades de fondo
│   │   ├── background-color
│   │   ├── background-image (url, gradientes)
│   │   ├── background-repeat (repeat, no-repeat, repeat-x, repeat-y, space, round)
│   │   ├── background-position (posiciones, valores, porcentajes)
│   │   ├── background-size (auto, cover, contain, valores)
│   │   ├── background-attachment (scroll, fixed, local)
│   │   ├── background-clip (border-box, padding-box, content-box, text)
│   │   ├── background-origin (border-box, padding-box, content-box)
│   │   ├── shorthand background
│   │   └── Múltiples fondos (separados por comas)
│   │
│   ├── Gradientes
│   │   ├── Gradientes lineales (linear-gradient)
│   │   │   ├── Dirección (to bottom, 45deg, etc.)
│   │   │   ├── Paradas de color (color stop)
│   │   │   ├── Gradientes con múltiples paradas
│   │   │   └── repeating-linear-gradient
│   │   ├── Gradientes radiales (radial-gradient)
│   │   │   ├── Forma (circle, ellipse)
│   │   │   ├── Posición (at center, at top left)
│   │   │   ├── Tamaño (closest-side, farthest-corner, etc.)
│   │   │   └── repeating-radial-gradient
│   │   ├── Gradientes cónicos (conic-gradient)
│   │   │   ├── Desde un ángulo (from 0deg)
│   │   │   └── Posición (at center)
│   │   └── Gradientes con transparencia (rgba, hsla)
│   │
│   ├── Sombras
│   │   ├── box-shadow
│   │   │   ├── offset-x, offset-y
│   │   │   ├── blur-radius (desenfoque)
│   │   │   ├── spread-radius (expansión)
│   │   │   ├── color
│   │   │   ├── inset (sombra interior)
│   │   │   └── Múltiples sombras
│   │   └── text-shadow (offset-x, offset-y, blur-radius, color)
│   │
│   ├── Máscaras y recortes
│   │   ├── clip-path (recortar elemento)
│   │   │   ├── Formas básicas (inset, circle, ellipse, polygon)
│   │   │   ├── Recortar a caja (margin-box, border-box, padding-box, content-box)
│   │   │   └── Recortar a SVG (url(#id))
│   │   ├── mask (máscara)
│   │   ├── background-blend-mode (modos de mezcla de fondos)
│   │   └── mix-blend-mode (modos de mezcla de elementos)
│   │
│   ├── Filtros
│   │   ├── filter (aplicar efectos)
│   │   │   ├── blur()
│   │   │   ├── brightness()
│   │   │   ├── contrast()
│   │   │   ├── grayscale()
│   │   │   ├── hue-rotate()
│   │   │   ├── invert()
│   │   │   ├── opacity()
│   │   │   ├── saturate()
│   │   │   ├── sepia()
│   │   │   ├── drop-shadow()
│   │   │   └── url() (filtros SVG)
│   │   └── backdrop-filter (filtros en el fondo detrás del elemento)
│   │
│   └── Efectos de borde
│       ├── outline (contorno, diferente de border)
│       │   ├── outline-width
│       │   ├── outline-style
│       │   ├── outline-color
│       │   └── outline-offset
│       └── box-decoration-break (comportamiento en elementos fragmentados)
│
├── Layout y Posicionamiento
│   ├── Flujo normal del documento
│   │   ├── Elementos de bloque (display: block)
│   │   ├── Elementos en línea (display: inline)
│   │   ├── Elementos en línea-bloque (display: inline-block)
│   │   └── display: none (ocultar elemento)
│   │
│   ├── Posicionamiento (position)
│   │   ├── static (por defecto)
│   │   ├── relative (relativo a su posición normal)
│   │   ├── absolute (absoluto respecto al ancestro posicionado)
│   │   ├── fixed (fijo respecto al viewport)
│   │   ├── sticky (híbrido relative/fixed)
│   │   ├── Propiedades de desplazamiento (top, right, bottom, left)
│   │   └── z-index (superposición y contexto de apilamiento)
│   │
│   ├── Flexbox (Diseño flexible unidimensional)
│   │   ├── Contenedor flex (display: flex, inline-flex)
│   │   ├── Eje principal y eje transversal
│   │   ├── Dirección (flex-direction: row, column, row-reverse, column-reverse)
│   │   ├── Ajuste de línea (flex-wrap: nowrap, wrap, wrap-reverse)
│   │   ├── shorthand flex-flow
│   │   ├── Alineación en eje principal (justify-content)
│   │   │   ├── flex-start, flex-end, center
│   │   │   ├── space-between, space-around, space-evenly
│   │   │   └── left, right (para flex-direction row)
│   │   ├── Alineación en eje transversal (align-items)
│   │   │   ├── stretch, flex-start, flex-end, center, baseline
│   │   │   └── align-self (para items individuales)
│   │   ├── Alineación de líneas múltiples (align-content)
│   │   ├── Espaciado (gap, row-gap, column-gap)
│   │   ├── Propiedades de los items
│   │   │   ├── order (cambiar orden visual)
│   │   │   ├── flex-grow (factor de crecimiento)
│   │   │   ├── flex-shrink (factor de encogimiento)
│   │   │   ├── flex-basis (tamaño base)
│   │   │   ├── shorthand flex (grow shrink basis)
│   │   │   └── align-self (alineación individual)
│   │   └── Casos de uso comunes
│   │       ├── Centrado perfecto
│   │       ├── Barras de navegación
│   │       ├── Tarjetas de altura igual
│   │       └── Distribución de espacio sobrante
│   │
│   ├── CSS Grid (Diseño bidimensional)
│   │   ├── Contenedor grid (display: grid, inline-grid)
│   │   ├── Conceptos fundamentales
│   │   │   ├── Pistas (filas y columnas)
│   │   │   ├── Líneas (grid lines)
│   │   │   ├── Celdas (grid cells)
│   │   │   └── Áreas (grid areas)
│   │   │
│   │   ├── Definición de la cuadrícula
│   │   │   ├── grid-template-columns
│   │   │   ├── grid-template-rows
│   │   │   ├── Unidades especiales: fr (fracción)
│   │   │   ├── repeat() (repetición)
│   │   │   ├── minmax() (tamaño mínimo-máximo)
│   │   │   ├── min-content, max-content, auto
│   │   │   ├── fit-content()
│   │   │   └── subgrid
│   │   │
│   │   ├── Ubicación de items
│   │   │   ├── Por líneas (grid-column-start, grid-column-end, grid-row-start, grid-row-end)
│   │   │   ├── Shorthands: grid-column, grid-row
│   │   │   ├── Span (abarcar)
│   │   │   └── Por áreas (grid-template-areas y grid-area)
│   │   │
│   │   ├── Grid implícito vs explícito
│   │   │   ├── grid-auto-rows
│   │   │   ├── grid-auto-columns
│   │   │   └── grid-auto-flow (dense, row, column)
│   │   │
│   │   ├── Alineación en Grid
│   │   │   ├── justify-items (alineación horizontal de items en celda)
│   │   │   ├── align-items (alineación vertical de items en celda)
│   │   │   ├── justify-self (alineación horizontal individual)
│   │   │   ├── align-self (alineación vertical individual)
│   │   │   ├── justify-content (alineación horizontal del grid)
│   │   │   ├── align-content (alineación vertical del grid)
│   │   │   └── place-items, place-self, place-content (shorthands)
│   │   │
│   │   └── Funciones y patrones
│   │       ├── repeat(auto-fill, minmax())
│   │       ├── repeat(auto-fit, minmax())
│   │       ├── Layouts de página completos
│   │       ├── Galerías responsivas
│   │       └── Diseño tipo "masonry" (con grid)
│   │
│   ├── Multi-columna
│   │   ├── column-count (número de columnas)
│   │   ├── column-width (ancho de columna)
│   │   ├── columns (shorthand)
│   │   ├── column-gap (espacio entre columnas)
│   │   ├── column-rule (línea entre columnas)
│   │   ├── column-span (elemento que abarca columnas)
│   │   └── break-inside (control de saltos)
│   │
│   └── Floats (legado)
│       ├── float (left, right, none)
│       ├── clear (left, right, both, none)
│       ├── Problemas de contenedor colapsado
│       └── Técnicas de clearfix
│
├── Diseño Responsivo
│   ├── Viewport
│   │   ├── meta viewport (width=device-width, initial-scale=1)
│   │   ├── viewport-fit (cover, contain, auto)
│   │   └── interactive-widget (ajuste para teclados virtuales)
│   │
│   ├── Media Queries
│   │   ├── Sintaxis @media
│   │   ├── Tipos de medio (all, screen, print)
│   │   ├── Operadores lógicos (and, or, not, only)
│   │   ├── Características de medios (media features)
│   │   │   ├── width, min-width, max-width
│   │   │   ├── height, min-height, max-height
│   │   │   ├── aspect-ratio
│   │   │   ├── orientation (portrait, landscape)
│   │   │   ├── resolution (resolución)
│   │   │   ├── hover (soporta hover)
│   │   │   ├── pointer (precisión del puntero)
│   │   │   ├── prefers-color-scheme (light, dark)
│   │   │   ├── prefers-reduced-motion (reduce)
│   │   │   ├── prefers-contrast (more, less)
│   │   │   ├── prefers-reduced-transparency
│   │   │   ├── prefers-reduced-data
│   │   │   ├── dynamic-range (standard, high)
│   │   │   └── forced-colors (modo de alto contraste)
│   │   │
│   │   └── Breakpoints comunes (mobile, tablet, desktop)
│   │
│   ├── Enfoques de diseño
│   │   ├── Mobile first (min-width)
│   │   ├── Desktop first (max-width)
│   │   └── Diseño fluido (sin breakpoints fijos)
│   │
│   ├── Imágenes responsivas
│   │   ├── max-width: 100% en imágenes
│   │   ├── Atributo srcset (con descriptores x y w)
│   │   ├── Atributo sizes (condiciones de visualización)
│   │   └── Elemento picture para art direction
│   │
│   ├── Tipografía responsiva
│   │   ├── Unidades vw para tamaño de fuente
│   │   ├── clamp() para tipografía fluida
│   │   └── viewport units con calc()
│   │
│   ├── Layouts responsivos
│   │   ├── Flex-wrap para componentes
│   │   ├── Grid con auto-fit/auto-fill
│   │   ├── Menús hamburguesa
│   │   └── Tablas responsivas (overflow, reorder)
│   │
│   └── Contenedores (Container Queries)
│       ├── @container
│       ├── container-type (size, inline-size)
│       ├── container-name
│       └── container query units (cqw, cqh, cqi, cqb, cqmin, cqmax)
│
├── Animaciones y Transiciones
│   ├── Transiciones (transition)
│   │   ├── transition-property (propiedades a animar)
│   │   ├── transition-duration (duración)
│   │   ├── transition-timing-function (curva de aceleración)
│   │   │   ├── ease, linear, ease-in, ease-out, ease-in-out
│   │   │   ├── steps() (animación por pasos)
│   │   │   └── cubic-bezier() (curva personalizada)
│   │   ├── transition-delay (retraso)
│   │   ├── shorthand transition
│   │   └── Propiedades que se pueden animar (animatable properties)
│   │
│   ├── Transformaciones (transform)
│   │   ├── Transformaciones 2D
│   │   │   ├── translate() / translateX() / translateY()
│   │   │   ├── rotate()
│   │   │   ├── scale() / scaleX() / scaleY()
│   │   │   ├── skew() / skewX() / skewY()
│   │   │   └── matrix() (transformación combinada)
│   │   ├── Transformaciones 3D
│   │   │   ├── translateZ() / translate3d()
│   │   │   ├── rotateX() / rotateY() / rotateZ() / rotate3d()
│   │   │   ├── scaleZ() / scale3d()
│   │   │   ├── perspective()
│   │   │   └── matrix3d()
│   │   ├── transform-origin (origen de la transformación)
│   │   ├── transform-style (preserve-3d, flat)
│   │   ├── perspective (profundidad)
│   │   ├── perspective-origin
│   │   └── backface-visibility (visible u oculto)
│   │
│   ├── Animaciones con @keyframes
│   │   ├── Definición de fotogramas clave (0%-100%, from-to)
│   │   ├── animation-name
│   │   ├── animation-duration
│   │   ├── animation-timing-function
│   │   ├── animation-delay
│   │   ├── animation-iteration-count (infinite, número)
│   │   ├── animation-direction (normal, reverse, alternate, alternate-reverse)
│   │   ├── animation-fill-mode (none, forwards, backwards, both)
│   │   ├── animation-play-state (running, paused)
│   │   ├── shorthand animation
│   │   └── Múltiples animaciones
│   │
│   ├── Rendimiento en animaciones
│   │   ├── Propiedades que afectan layout (width, height, top, left)
│   │   ├── Propiedades que solo afectan paint (color, background)
│   │   ├── Propiedades que solo afectan compositor (transform, opacity)
│   │   ├── will-change (optimización)
│   │   └── Uso de transform y opacity para animaciones suaves
│   │
│   └── Animaciones de scroll
│       ├── scroll-timeline (concepto)
│       └── view-timeline (concepto)
│
├── Pseudoclases y Pseudoelementos
│   ├── Pseudoclases de interacción
│   │   ├── :hover (ratón encima)
│   │   ├── :active (momento de clic)
│   │   ├── :focus (enfocado)
│   │   ├── :focus-visible (enfocado por teclado)
│   │   ├── :focus-within (elemento o sus hijos enfocados)
│   │   ├── :target (elemento con ID igual al hash)
│   │   └── :visited (enlace visitado)
│   │
│   ├── Pseudoclases de posición y estructura
│   │   ├── :first-child
│   │   ├── :last-child
│   │   ├── :first-of-type
│   │   ├── :last-of-type
│   │   ├── :nth-child()
│   │   ├── :nth-last-child()
│   │   ├── :nth-of-type()
│   │   ├── :nth-last-of-type()
│   │   ├── :only-child
│   │   ├── :only-of-type
│   │   ├── :empty
│   │   └── :root (elemento raíz, html)
│   │
│   ├── Pseudoclases de formulario
│   │   ├── :checked
│   │   ├── :indeterminate
│   │   ├── :default
│   │   ├── :disabled / :enabled
│   │   ├── :read-only / :read-write
│   │   ├── :required / :optional
│   │   ├── :valid / :invalid
│   │   ├── :in-range / :out-of-range
│   │   └── :user-valid / :user-invalid (interacción usuario)
│   │
│   ├── Pseudoclases funcionales
│   │   ├── :is() (selector con lista, el más específico)
│   │   ├── :where() (selector con lista, sin especificidad)
│   │   ├── :has() (selector relacional, padre con hijo que cumple)
│   │   ├── :not() (negación)
│   │   └── :lang() (idioma)
│   │
│   └── Pseudoelementos
│       ├── ::before (antes del contenido)
│       ├── ::after (después del contenido)
│       │   └── Propiedad content (obligatoria)
│       │       ├── text (string)
│       │       ├── url() (imagen)
│       │       ├── attr() (atributo del elemento)
│       │       ├── open-quote / close-quote
│       │       └── counter() / counters()
│       ├── ::first-letter (primera letra)
│       ├── ::first-line (primera línea)
│       ├── ::selection (texto seleccionado)
│       ├── ::placeholder (placeholder en inputs)
│       ├── ::marker (marcador de lista)
│       ├── ::backdrop (fondo de elementos en pantalla completa)
│       └── ::file-selector-button (botón de input file)
│
├── Arquitectura y Metodologías CSS
│   ├── Especificidad en profundidad
│   │   ├── Cálculo (inline > ID > clase > elemento)
│   │   ├── Especificidad de pseudoclases
│   │   ├── Especificidad de pseudoelementos
│   │   ├── Especificidad de :is() y :where()
│   │   └── Herramientas de depuración
│   │
│   ├── Cascada
│   │   ├── Orden de origen (el último gana)
│   │   ├── Importancia (!important)
│   │   ├── Origen de estilos (agente usuario, usuario, autor)
│   │   └── Capas (@layer)
│   │
│   ├── Metodologías de nomenclatura
│   │   ├── BEM (Bloque, Elemento, Modificador)
│   │   │   ├── Bloque (bloque)
│   │   │   ├── Elemento (bloque__elemento)
│   │   │   └── Modificador (bloque--modificador, bloque__elemento--modificador)
│   │   ├── SMACSS (categorías)
│   │   ├── OOCSS (separación estructura y piel)
│   │   └── Utility-first (como Tailwind, conceptualmente)
│   │
│   ├── Organización de archivos
│   │   ├── Estructura modular
│   │   ├── Separación por componentes
│   │   ├── Archivos base, layout, componentes, páginas
│   │   └── Patrón 7-1 (concepto)
│   │
│   └── Capas CSS (@layer)
│       ├── Definición de capas
│       ├── Orden de capas
│       └── Importar a capas
│
└── Variables CSS (Custom Properties)
    ├── Definición (--nombre-variable)
    ├── Uso (var(--nombre-variable))
    ├── Valor por defecto (var(--nombre, fallback))
    ├── Ámbito (scope) de variables
    ├── Herencia de variables
    ├── Variables en media queries
    ├── Manipulación con JavaScript
    └── Temas dinámicos con variables

```

---

# JAVASCRIPT (LENGUAJE DE PROGRAMACIÓN)

## Programación Procedimental
```
JavaScript
├── Programación Procedimental
│   ├── Sintaxis básica
│   │   ├── Declaraciones y expresiones
│   │   ├── Punto y coma (opcional, pero recomendado)
│   │   ├── Comentarios (// y /* */)
│   │   └── Modo estricto ("use strict")
│   │
│   ├── Variables
│   │   ├── Declaración (var, let, const)
│   │   │   ├── var (function scope, hoisting, redeclarable)
│   │   │   ├── let (block scope, hoisting temporal muerto, no redeclarable)
│   │   │   └── const (block scope, no reasignable, mutable si objeto)
│   │   ├── Hoisting (elevación de declaraciones)
│   │   ├── Temporal Dead Zone (zona muerta temporal)
│   │   ├── Ámbito (scope)
│   │   │   ├── Global scope
│   │   │   ├── Function scope
│   │   │   ├── Block scope
│   │   │   └── Lexical scope
│   │   └── Buenas prácticas en nombrado (camelCase, descriptivo)
│   │
│   ├── Tipos de datos
│   │   ├── Primitivos (inmutables)
│   │   │   ├── number (enteros, decimales, NaN, Infinity)
│   │   │   ├── string (cadenas de texto)
│   │   │   ├── boolean (true, false)
│   │   │   ├── undefined (valor no asignado)
│   │   │   ├── null (valor nulo intencional)
│   │   │   ├── symbol (identificador único)
│   │   │   └── bigint (números grandes)
│   │   ├── Objetos (referencias, mutables)
│   │   │   ├── Object literal
│   │   │   ├── Array
│   │   │   ├── Function
│   │   │   ├── Date
│   │   │   ├── RegExp
│   │   │   ├── Map, Set, WeakMap, WeakSet
│   │   │   └── ... (otros objetos nativos)
│   │   ├── typeof (operador para conocer tipo)
│   │   ├── instanceof (para verificar instancia)
│   │   └── Conversión de tipos (type coercion)
│   │       ├── Conversión explícita (Number(), String(), Boolean())
│   │       ├── Conversión implícita (operadores)
│   │       ├── Truthy y Falsy (valores que evalúan como booleanos)
│   │       └── Comparación (== vs ===)
│   │
│   ├── Operadores
│   │   ├── Asignación (=, +=, -=, *=, /=, %=, **=)
│   │   ├── Aritméticos (+, -, *, /, %, **, ++, --)
│   │   ├── Comparación (==, ===, !=, !==, >, <, >=, <=)
│   │   ├── Lógicos (&&, ||, !)
│   │   ├── Bitwise (&, |, ^, ~, <<, >>, >>>)
│   │   ├── String (+ para concatenación)
│   │   ├── Ternario (condición ? exprSiTrue : exprSiFalse)
│   │   ├── Nullish coalescing (??) (null o undefined)
│   │   ├── Optional chaining (?.) (acceso seguro)
│   │   ├── in (propiedad en objeto)
│   │   ├── instanceof (instancia de clase)
│   │   └── typeof (tipo de valor)
│   │
│   ├── Estructuras de control condicionales
│   │   ├── if / else if / else
│   │   ├── switch (case, break, default)
│   │   │   └── Fallthrough intencional
│   │   └── Operador ternario (para expresiones simples)
│   │
│   ├── Bucles (loops)
│   │   ├── while (mientras condición)
│   │   ├── do...while (ejecuta al menos una vez)
│   │   ├── for clásico (inicialización; condición; incremento)
│   │   ├── for...in (itera sobre propiedades enumerables de objetos)
│   │   ├── for...of (itera sobre valores de iterables)
│   │   ├── for await...of (para iterables asíncronos)
│   │   ├── break (salir del bucle)
│   │   ├── continue (saltar a siguiente iteración)
│   │   ├── labeled statements (etiquetas para break/continue)
│   │   └── Bucles infinitos (cómo evitarlos)
│   │
│   └── Funciones (básico)
│       ├── Declaración de función (function declaration)
│       │   ├── Hoisting de declaraciones
│       │   └── Parámetros y argumentos
│       ├── Expresión de función (function expression)
│       │   ├── Anónimas y nombradas
│       │   └── No hoisting (solo variable)
│       ├── Parámetros
│       │   ├── Parámetros por defecto (ES6)
│       │   ├── Rest parameters (...args)
│       │   ├── arguments (objeto similar a array, solo en funciones tradicionales)
│       │   └── Pasar por valor vs por referencia
│       ├── Return
│       │   ├── Valor de retorno
│       │   └── Undefined implícito si no hay return
│       └── Funciones como ciudadanos de primera clase
│           ├── Asignar a variables
│           ├── Pasar como argumento (callbacks)
│           └── Retornar desde otra función (closures)
│
├── Programación Orientada a Objetos
│   ├── Objetos literales
│   │   ├── Creación con {} (object literal)
│   │   ├── Propiedades (clave: valor)
│   │   ├── Métodos (funciones como propiedades)
│   │   ├── Acceso a propiedades (dot notation, bracket notation)
│   │   ├── Propiedades calculadas (computed properties)
│   │   ├── Shorthand properties (cuando variable y propiedad tienen mismo nombre)
│   │   ├── Métodos shorthand (nombreMetodo() {})
│   │   ├── Getters y Setters (get, set)
│   │   ├── Recorrer propiedades (for...in, Object.keys, Object.values, Object.entries)
│   │   ├── Comprobar propiedad (hasOwnProperty, in operator)
│   │   └── Eliminar propiedad (delete)
│   │
│   ├── Prototipos (prototype)
│   │   ├── Cadena de prototipos (prototype chain)
│   │   ├── __proto__ (obsoleto, no usar)
│   │   ├── Object.getPrototypeOf() / Object.setPrototypeOf()
│   │   ├── Propiedades propias vs heredadas
│   │   ├── Object.create() (crear objeto con prototipo específico)
│   │   ├── Constructor functions (funciones constructoras)
│   │   │   ├── new keyword (crea instancia)
│   │   │   ├── this en constructores
│   │   │   └── prototype para métodos compartidos
│   │   └── Herencia prototípica
│   │
│   ├── Clases (ES6+)
│   │   ├── Declaración de clase (class)
│   │   ├── Constructor (constructor)
│   │   ├── Métodos de instancia
│   │   ├── Propiedades de instancia (public class fields)
│   │   ├── Métodos estáticos (static)
│   │   ├── Propiedades estáticas (static fields)
│   │   ├── Propiedades privadas (#campoPrivado)
│   │   ├── Métodos privados (#metodoPrivado)
│   │   ├── Getters y setters en clases
│   │   ├── Herencia (extends)
│   │   │   ├── super() para llamar al constructor padre
│   │   │   ├── super.metodoPadre() para llamar métodos
│   │   │   └── Sobrescritura de métodos (override)
│   │   ├── instanceof con clases
│   │   └── Clases como "syntactic sugar" sobre prototipos
│   │
│   ├── this keyword
│   │   ├── this en ámbito global
│   │   ├── this en función (depende de cómo se llama)
│   │   ├── this en método de objeto (el objeto)
│   │   ├── this en constructor (la nueva instancia)
│   │   ├── this en evento (el elemento DOM)
│   │   ├── this en arrow functions (heredado del ámbito léxico)
│   │   ├── Pérdida de this (cuando se pasa como callback)
│   │   └── Control explícito de this
│   │       ├── call() (llamar con this específico)
│   │       ├── apply() (igual pero con array de args)
│   │       └── bind() (crear nueva función con this fijado)
│   │
│   └── Encapsulación y abstracción
│       ├── Closures (cierres) para datos privados
│       ├── Module pattern (IIFE + closures)
│       ├── Propiedades privadas con #
│       └── Getters/setters para control de acceso
│
├── Programación Funcional
│   ├── Conceptos fundamentales
│   │   ├── Funciones puras (misma entrada, misma salida, sin efectos secundarios)
│   │   ├── Inmutabilidad (no modificar datos, crear nuevos)
│   │   ├── Efectos secundarios (side effects)
│   │   ├── Composición de funciones
│   │   └── Declarativo vs imperativo
│   │
│   ├── Funciones de orden superior
│   │   ├── Funciones que reciben funciones
│   │   ├── Funciones que retornan funciones
│   │   └── Ejemplos comunes (map, filter, reduce)
│   │
│   ├── Arrow functions
│   │   ├── Sintaxis compacta
│   │   ├── Return implícito (sin llaves)
│   │   ├── No tienen su propio this (heredan)
│   │   ├── No tienen arguments
│   │   ├── No pueden ser constructoras (sin new)
│   │   └── Cuándo usarlas vs function tradicional
│   │
│   ├── Métodos de array funcionales
│   │   ├── forEach (ejecutar función por cada elemento)
│   │   ├── map (transformar cada elemento, nuevo array)
│   │   ├── filter (filtrar elementos, nuevo array)
│   │   ├── reduce (reducir a un solo valor)
│   │   ├── reduceRight (igual pero de derecha a izquierda)
│   │   ├── find (encontrar primer elemento que cumple)
│   │   ├── findIndex (índice del primer elemento)
│   │   ├── findLast / findLastIndex (desde el final)
│   │   ├── some (al menos uno cumple)
│   │   ├── every (todos cumplen)
│   │   ├── flat (aplanar array)
│   │   ├── flatMap (map + flat)
│   │   └── Encadenamiento de métodos (chaining)
│   │
│   ├── Inmutabilidad práctica
│   │   ├── Spread operator (...) para copias
│   │   │   ├── Copiar arrays (nuevoArray = [...array])
│   │   │   ├── Añadir elementos (nuevoArray = [...array, elemento])
│   │   │   ├── Copiar objetos (nuevoObj = {...obj})
│   │   │   └── Actualizar propiedades (nuevoObj = {...obj, prop: nuevoValor})
│   │   ├── Object.assign() para copias
│   │   ├── Array.from() para copias de array-like
│   │   ├── concat() para arrays (no mutable)
│   │   ├── slice() para extraer sin modificar
│   │   └── toSorted, toReversed, toSpliced (métodos que retornan copia)
│   │
│   └── Currying y composición
│       ├── Currying (transformar función con múltiples args en secuencia de funciones)
│       ├── Partial application (aplicación parcial)
│       ├── compose (composición de funciones)
│       └── pipe (composición en orden de ejecución)
│
├── Estructuras de Datos
│   ├── Arrays
│   │   ├── Creación (literales [], Array constructor, Array.from, Array.of)
│   │   ├── Propiedades (length)
│   │   ├── Acceso por índice
│   │   ├── Métodos de adición/eliminación
│   │   │   ├── push (añadir al final)
│   │   │   ├── pop (eliminar del final)
│   │   │   ├── unshift (añadir al inicio)
│   │   │   └── shift (eliminar del inicio)
│   │   ├── Métodos de modificación
│   │   │   ├── splice (eliminar/insertar en cualquier posición)
│   │   │   ├── fill (rellenar con valor)
│   │   │   ├── copyWithin (copiar dentro del mismo array)
│   │   │   ├── reverse (invertir orden)
│   │   │   └── sort (ordenar)
│   │   ├── Métodos de acceso (sin modificar)
│   │   │   ├── concat (concatenar)
│   │   │   ├── slice (extraer porción)
│   │   │   ├── includes (contiene valor)
│   │   │   ├── indexOf / lastIndexOf (posición)
│   │   │   └── join (convertir a string)
│   │   ├── Métodos de iteración (funcionales)
│   │   ├── Arrays multidimensionales
│   │   ├── Arrays y destructuring
│   │   └── Matrices (arrays de arrays)
│   │
│   ├── Objetos (visto en POO)
│   │
│   ├── Map
│   │   ├── Creación (new Map())
│   │   ├── set (añadir par clave-valor)
│   │   ├── get (obtener por clave)
│   │   ├── has (comprobar clave)
│   │   ├── delete (eliminar por clave)
│   │   ├── clear (vaciar)
│   │   ├── size (tamaño)
│   │   ├── keys, values, entries (iteradores)
│   │   ├── forEach (iterar)
│   │   └── Diferencias con objetos (claves de cualquier tipo, orden, tamaño)
│   │
│   ├── Set
│   │   ├── Creación (new Set())
│   │   ├── add (añadir valor)
│   │   ├── has (comprobar existencia)
│   │   ├── delete (eliminar valor)
│   │   ├── clear (vaciar)
│   │   ├── size (tamaño)
│   │   ├── keys, values, entries (iteradores)
│   │   ├── forEach (iterar)
│   │   └── Eliminar duplicados de arrays ([...new Set(array)])
│   │
│   ├── WeakMap y WeakSet
│   │   ├── Claves débiles (solo objetos)
│   │   ├── No evitan garbage collection
│   │   ├── No son iterables
│   │   ├── No tienen size
│   │   └── Casos de uso (metadatos, caché, privacidad)
│   │
│   └── Typed Arrays (para datos binarios)
│       ├── ArrayBuffer
│       ├── DataView
│       └── TypedArray views (Int8Array, Uint8Array, etc.)
│
├── Manipulación del DOM
│   ├── Selección de elementos
│   │   ├── document.getElementById()
│   │   ├── document.getElementsByClassName() (HTMLCollection)
│   │   ├── document.getElementsByTagName() (HTMLCollection)
│   │   ├── document.querySelector() (primer elemento, selector CSS)
│   │   ├── document.querySelectorAll() (NodeList)
│   │   └── Diferencias entre HTMLCollection y NodeList
│   │
│   ├── Recorrer el DOM (navegación)
│   │   ├── parentNode / parentElement
│   │   ├── childNodes / children
│   │   ├── firstChild / firstElementChild
│   │   ├── lastChild / lastElementChild
│   │   ├── nextSibling / nextElementSibling
│   │   ├── previousSibling / previousElementSibling
│   │   ├── closest() (ancestro que cumple selector)
│   │   └── contains() (comprobar si contiene)
│   │
│   ├── Modificar contenido
│   │   ├── textContent (solo texto)
│   │   ├── innerHTML (HTML, peligro XSS)
│   │   ├── innerText (texto con estilos aplicados)
│   │   ├── outerHTML (incluye el propio elemento)
│   │   └── insertAdjacentText / insertAdjacentHTML
│   │
│   ├── Modificar atributos
│   │   ├── getAttribute() / setAttribute()
│   │   ├── removeAttribute()
│   │   ├── hasAttribute()
│   │   ├── Atributos directos (element.id, element.className, element.src, etc.)
│   │   ├── classList (API moderna para clases)
│   │   │   ├── add()
│   │   │   ├── remove()
│   │   │   ├── toggle()
│   │   │   ├── contains()
│   │   │   └── replace()
│   │   └── dataset (data-* attributes)
│   │
│   ├── Modificar estilos
│   │   ├── style propiedad (estilos en línea)
│   │   ├── getComputedStyle() (estilos calculados)
│   │   └── Mejor práctica: modificar clases CSS
│   │
│   ├── Crear e insertar elementos
│   │   ├── document.createElement()
│   │   ├── document.createTextNode()
│   │   ├── cloneNode() (clonar elemento)
│   │   ├── appendChild() (añadir al final)
│   │   ├── insertBefore() (insertar antes de otro)
│   │   ├── append() (moderno, múltiple)
│   │   ├── prepend() (al inicio)
│   │   ├── after() / before() (insertar adyacente)
│   │   ├── replaceChild() / replaceWith()
│   │   ├── insertAdjacentElement()
│   │   └── innerHTML vs creación manual (rendimiento y seguridad)
│   │
│   ├── Eliminar elementos
│   │   ├── removeChild()
│   │   ├── remove() (directo)
│   │   └── Vaciar elemento (innerHTML = "")
│   │
│   ├── Dimensiones y posiciones
│   │   ├── offsetWidth / offsetHeight (incluye border, padding)
│   │   ├── clientWidth / clientHeight (incluye padding, sin border)
│   │   ├── scrollWidth / scrollHeight (tamaño total con scroll)
│   │   ├── offsetTop / offsetLeft (posición relativa a offsetParent)
│   │   ├── clientTop / clientLeft (borde superior/izquierdo)
│   │   ├── scrollTop / scrollLeft (posición de scroll)
│   │   ├── getBoundingClientRect() (posición relativa al viewport)
│   │   ├── elementFromPoint() (elemento en coordenadas)
│   │   └── matches() (comprobar si coincide con selector)
│   │
│   └── Actualizaciones eficientes
│       ├── Fragmentos de documento (DocumentFragment)
│       ├── requestAnimationFrame para animaciones
│       └── Evitar reflows/repaints innecesarios
│
├── Manejo de Eventos
│   ├── Tipos de eventos
│   │   ├── Eventos de ratón
│   │   │   ├── click
│   │   │   ├── dblclick
│   │   │   ├── mousedown / mouseup
│   │   │   ├── mousemove
│   │   │   ├── mouseenter / mouseleave (no burbujean)
│   │   │   ├── mouseover / mouseout (burbujean)
│   │   │   ├── contextmenu (clic derecho)
│   │   │   └── wheel (scroll de rueda)
│   │   ├── Eventos de teclado
│   │   │   ├── keydown
│   │   │   ├── keyup
│   │   │   └── keypress (obsoleto)
│   │   ├── Eventos de formulario
│   │   │   ├── submit
│   │   │   ├── reset
│   │   │   ├── change
│   │   │   ├── input
│   │   │   ├── focus / blur
│   │   │   ├── focusin / focusout (burbujean)
│   │   │   └── invalid
│   │   ├── Eventos de ventana y documento
│   │   │   ├── load
│   │   │   ├── DOMContentLoaded
│   │   │   ├── beforeunload / unload
│   │   │   ├── resize
│   │   │   ├── scroll
│   │   │   ├── hashchange
│   │   │   └── popstate (historial)
│   │   ├── Eventos de portapapeles
│   │   │   ├── copy
│   │   │   ├── cut
│   │   │   └── paste
│   │   ├── Eventos de arrastrar y soltar (drag & drop)
│   │   ├── Eventos de medios (video/audio)
│   │   ├── Eventos de animación CSS
│   │   └── Eventos de transición CSS
│   │
│   ├── Registro de eventos
│   │   ├── Atributos HTML (onclick="funcion()") → NO RECOMENDADO
│   │   ├── Propiedades del elemento (element.onclick = funcion)
│   │   └── addEventListener() (recomendado)
│   │       ├── Tipo de evento
│   │       ├── Función manejadora
│   │       ├── Opciones (useCapture, once, passive)
│   │       └── removeEventListener() (requiere misma función)
│   │
│   ├── El objeto Event
│   │   ├── Propiedades
│   │   │   ├── type (tipo de evento)
│   │   │   ├── target (elemento que disparó el evento)
│   │   │   ├── currentTarget (elemento donde está el listener)
│   │   │   ├── timeStamp
│   │   │   ├── defaultPrevented
│   │   │   └── Propiedades específicas según tipo (clientX, key, etc.)
│   │   ├── Métodos
│   │   │   ├── preventDefault() (evitar comportamiento por defecto)
│   │   │   ├── stopPropagation() (evitar bubbling)
│   │   │   └── stopImmediatePropagation() (evitar otros listeners)
│   │
│   ├── Fases del evento
│   │   ├── Captura (fase 1, de window al target)
│   │   ├── Target (fase 2)
│   │   ├── Burbujeo (fase 3, del target al window)
│   │   └── Uso de useCapture en addEventListener
│   │
│   ├── Delegación de eventos
│   │   ├── Concepto (listener en padre para hijos dinámicos)
│   │   ├── Identificar target específico
│   │   └── Ventajas (rendimiento, elementos dinámicos)
│   │
│   └── Eventos personalizados
│       ├── new CustomEvent()
│       ├── dispatchEvent()
│       └── Pasar datos con detail
│
├── Programación Asíncrona
│   ├── JavaScript síncrono vs asíncrono
│   │   ├── Single-threaded (un solo hilo)
│   │   ├── Bloqueante vs no bloqueante
│   │   └── Operaciones asíncronas comunes (timers, peticiones, eventos)
│   │
│   ├── Event Loop (Bucle de eventos)
│   │   ├── Call Stack (pila de llamadas)
│   │   ├── Web APIs (APIs del navegador)
│   │   ├── Task Queue (cola de tareas / macrotasks)
│   │   ├── Microtask Queue (cola de microtareas)
│   │   ├── Cómo funciona el event loop
│   │   └── Prioridad: microtasks > tasks
│   │
│   ├── Temporizadores
│   │   ├── setTimeout()
│   │   │   ├── Retraso mínimo (delay)
│   │   │   ├── Parámetros adicionales
│   │   │   └── clearTimeout()
│   │   ├── setInterval()
│   │   │   ├── Ejecución repetida
│   │   │   ├── Problemas (acumulación)
│   │   │   └── clearInterval()
│   │   └── requestAnimationFrame()
│   │       ├── Para animaciones
│   │       ├── Sincronizado con refresco de pantalla
│   │       └── cancelAnimationFrame()
│   │
│   ├── Callbacks
│   │   ├── Patrón básico
│   │   ├── Callback hell (pirámide de la perdición)
│   │   ├── Manejo de errores con callbacks (error-first pattern)
│   │   └── Limitaciones (inversión de control, anidamiento)
│   │
│   ├── Promesas (Promises)
│   │   ├── Estados (pending, fulfilled, rejected)
│   │   ├── Crear promesa (new Promise())
│   │   │   ├── resolve y reject
│   │   │   └── Ejecutor se ejecuta inmediatamente
│   │   ├── Consumir promesas
│   │   │   ├── then() (manejar éxito)
│   │   │   │   ├── Recibe valor resuelto
│   │   │   │   └── Retorna nueva promesa (chaining)
│   │   │   ├── catch() (manejar error)
│   │   │   ├── finally() (siempre se ejecuta)
│   │   │   └── Chaining (encadenamiento)
│   │   ├── Propagación de errores
│   │   ├── Métodos estáticos
│   │   │   ├── Promise.resolve()
│   │   │   ├── Promise.reject()
│   │   │   ├── Promise.all() (todas resueltas, falla si una falla)
│   │   │   ├── Promise.allSettled() (todas, con estado individual)
│   │   │   ├── Promise.race() (la primera en resolverse o rechazarse)
│   │   │   └── Promise.any() (la primera en resolverse, falla si todas fallan)
│   │   └── thenable (objetos con método then)
│   │
│   ├── Async / Await
│   │   ├── Declarar función async (siempre retorna promesa)
│   │   ├── await (pausa hasta que la promesa se resuelva)
│   │   │   ├── Solo dentro de funciones async
│   │   │   └── Puede await cualquier thenable
│   │   ├── Manejo de errores con try/catch
│   │   ├── Async/await con Promise.all (ejecución paralela)
│   │   ├── Bucles asíncronos (for await...of)
│   │   └── Comparación con then/catch
│   │
│   └── Tareas en segundo plano
│       ├── Web Workers (concepto)
│       │   ├── Worker dedicado
│       │   ├── Comunicación con postMessage
│       │   └── Limitaciones (sin acceso a DOM)
│       └── Service Workers (concepto, para PWA)
│
├── Comunicación con APIs
│   ├── Fetch API
│   │   ├── Sintaxis básica (fetch(url))
│   │   ├── Objeto Request
│   │   ├── Objeto Response
│   │   │   ├── ok (status 200-299)
│   │   │   ├── status / statusText
│   │   │   ├── headers
│   │   │   ├── json() (procesar como JSON)
│   │   │   ├── text() (como texto)
│   │   │   ├── blob() (como binario)
│   │   │   ├── arrayBuffer() (como buffer)
│   │   │   └── formData() (como FormData)
│   │   ├── Configuración de petición
│   │   │   ├── method (GET, POST, PUT, DELETE, PATCH)
│   │   │   ├── headers (cabeceras)
│   │   │   ├── body (cuerpo)
│   │   │   ├── mode (cors, no-cors, same-origin)
│   │   │   ├── credentials (include, same-origin, omit)
│   │   │   ├── cache (control de caché)
│   │   │   └── signal (para abortar)
│   │   ├── Manejo de errores
│   │   │   ├── Fetch solo rechaza por error de red
│   │   │   ├── Verificar response.ok
│   │   │   └── Lanzar error manualmente si es necesario
│   │   └── Abortar petición (AbortController)
│   │
│   ├── XMLHttpRequest (legado)
│   │   ├── Crear instancia
│   │   ├── open() (configurar)
│   │   ├── send() (enviar)
│   │   ├── Eventos (onload, onerror, onprogress)
│   │   ├── readyState
│   │   └── responseType
│   │
│   ├── JSON
│   │   ├── Sintaxis JSON (objetos, arrays, strings, numbers, booleanos, null)
│   │   ├── JSON.stringify() (convertir objeto JS a string JSON)
│   │   │   ├── Replacer (filtrar propiedades)
│   │   │   └── Space (formato legible)
│   │   ├── JSON.parse() (convertir string JSON a objeto JS)
│   │   │   └── Reviver (transformar valores)
│   │   └── Manejo de fechas y otros tipos (serialización personalizada)
│   │
│   ├── FormData
│   │   ├── Crear desde formulario (new FormData(form))
│   │   ├── append(), set(), get(), getAll(), delete(), has()
│   │   ├── Enviar con fetch (body: formData)
│   │   └── Content-Type automático (multipart/form-data)
│   │
│   ├── CORS (Cross-Origin Resource Sharing)
│   │   ├── Same-origin policy (política del mismo origen)
│   │   ├── Peticiones simples vs preflight
│   │   ├── Cabeceras CORS (Access-Control-Allow-Origin, etc.)
│   │   ├── Credenciales (withCredentials)
│   │   └── Soluciones (proxy, CORS en servidor)
│   │
│   └── WebSockets (concepto básico)
│       ├── Conexión persistente bidireccional
│       ├── WebSocket API
│       └── Eventos (open, message, error, close)
│
├── Almacenamiento en Cliente
│   ├── Cookies
│   │   ├── document.cookie
│   │   ├── Leer cookies
│   │   ├── Escribir cookies (clave=valor; expires; path; domain; secure; samesite)
│   │   ├── Eliminar cookies
│   │   ├── Atributos (HttpOnly, Secure, SameSite)
│   │   └── Limitaciones (tamaño, enviadas en cada petición)
│   │
│   ├── Web Storage
│   │   ├── localStorage
│   │   │   ├── setItem(), getItem(), removeItem(), clear()
│   │   │   ├── key() (acceso por índice)
│   │   │   ├── length
│   │   │   ├── Persistente (no expira)
│   │   │   └── Solo strings (usar JSON)
│   │   ├── sessionStorage
│   │   │   ├── Igual que localStorage
│   │   │   └── Por sesión (se borra al cerrar pestaña)
│   │   └── Evento storage (cuando cambia en otra pestaña)
│   │
│   ├── IndexedDB (concepto y básico)
│   │   ├── Base de datos NoSQL en el navegador
│   │   ├── Objetos store (tablas)
│   │   ├── Índices (búsquedas)
│   │   ├── Transacciones
│   │   ├── Asíncrono con eventos
│   │   └── Cuándo usarlo (grandes volúmenes de datos)
│   │
│   └── Cache API (Service Workers)
│       ├── Cache storage
│       ├── caches.open(), caches.match()
│       └── Para PWA y offline
│
├── Módulos y Organización
│   ├── Módulos ES6
│   │   ├── export (named exports, default export)
│   │   │   ├── Exportar declaraciones
│   │   │   ├── Exportar lista al final
│   │   │   └── Renombrar con as
│   │   ├── import
│   │   │   ├── Importar named (con {})
│   │   │   ├── Importar default (sin llaves)
│   │   │   ├── Importar todo (import * as alias)
│   │   │   ├── Renombrar con as
│   │   │   └── Importar para efectos secundarios (import './modulo.js')
│   │   ├── Módulos en navegador (type="module")
│   │   │   ├── Diferido por defecto (defer)
│   │   │   ├── Ámbito de módulo
│   │   │   ├── Import maps
│   │   │   └── CORS en módulos
│   │   └── Módulos dinámicos (import() as function)
│   │       ├── Retorna promesa
│   │       └── Lazy loading
│   │
│   ├── Module Pattern (IIFE)
│   │   ├── Immediately Invoked Function Expression
│   │   ├── Encapsulación con closures
│   │   ├── Retornar API pública
│   │   └── Variables privadas
│   │
│   └── Bundling (concepto, sin herramientas)
│       ├── Por qué necesitamos bundlers
│       ├── Dependencias entre módulos
│       └── Árbol de módulos
│
├── Manejo de Errores y Depuración
│   ├── Tipos de errores
│   │   ├── SyntaxError (error de sintaxis)
│   │   ├── ReferenceError (variable no definida)
│   │   ├── TypeError (operación en tipo incorrecto)
│   │   ├── RangeError (valor fuera de rango)
│   │   ├── URIError (funciones URI mal usadas)
│   │   ├── EvalError (raro, con eval)
│   │   └── Error personalizado
│   │
│   ├── try/catch/finally
│   │   ├── try (código que puede lanzar error)
│   │   ├── catch (manejar error)
│   │   │   ├── Parámetro de error
│   │   │   └── catch sin parámetro (opcional)
│   │   ├── finally (siempre se ejecuta)
│   │   └── Anidamiento
│   │
│   ├── throw
│   │   ├── Lanzar error (throw new Error())
│   │   ├── Lanzar cualquier cosa (throw "mensaje")
│   │   └── Relanzar errores en catch
│   │
│   ├── Error personalizado (class MyError extends Error)
│   │
│   └── Depuración
│       ├── console.log(), console.warn(), console.error()
│       ├── console.table() (para arrays/objetos)
│       ├── console.time() / console.timeEnd() (medir rendimiento)
│       ├── console.trace() (pila de llamadas)
│       ├── console.group() / console.groupEnd() (agrupar)
│       ├── console.count() (contar veces)
│       ├── console.assert() (solo si falla)
│       ├── debugger (pausa en DevTools)
│       ├── Breakpoints en Sources
│       ├── Step over, step into, step out
│       ├── Watch expressions
│       └── Call stack
│
├── Patrones y Buenas Prácticas
│   ├── Debouncing y Throttling
│   │   ├── Debounce (ejecutar después de un tiempo sin nuevas llamadas)
│   │   ├── Throttle (ejecutar como máximo cada X tiempo)
│   │   └── Implementaciones manuales (con setTimeout)
│   │
│   ├── Closures (cierres) avanzados
│   │   ├── Fábricas de funciones
│   │   ├── Encapsulación de datos privados
│   │   ├── Módulos con closures
│   │   └── Currying con closures
│   │
│   ├── IIFE (Immediately Invoked Function Expression)
│   │   ├── Crear ámbito aislado
│   │   ├── Module pattern
│   │   └── Patrón de módulo revelador
│   │
│   ├── Memoización (caching de resultados)
│   │   ├── Funciones memoizadas
│   │   ├── Optimización de cálculos costosos
│   │   └── Implementación con closures y objetos
│   │
│   ├── Patrón Singleton (concepto)
│   │
│   ├── Patrón Observador (Observer)
│   │   ├── Eventos como implementación
│   │   └── Pub/Sub simple
│   │
│   ├── Separación de responsabilidades
│   │   ├── HTML (estructura)
│   │   ├── CSS (presentación)
│   │   └── JS (comportamiento)
│   │
│   └── Código limpio
│       ├── Nombres descriptivos
│       ├── Funciones pequeñas (una sola responsabilidad)
│       ├── Evitar efectos secundarios
│       ├── Comentar el por qué, no el qué
│       └── Consistencia en el estilo
│
└── APIs del Navegador (Web APIs)
    ├── Window (objeto global)
    │   ├── window.innerWidth / innerHeight
    │   ├── window.scrollX / scrollY
    │   ├── window.open(), window.close()
    │   ├── window.alert(), window.confirm(), window.prompt()
    │   ├── window.setTimeout / setInterval (vistos)
    │   ├── window.requestAnimationFrame
    │   ├── window.location (URL actual)
    │   ├── window.history (navegación)
    │   ├── window.navigator (info del navegador)
    │   └── window.localStorage / sessionStorage
    │
    ├── Document (visto en DOM)
    │
    ├── Navigator
    │   ├── navigator.userAgent
    │   ├── navigator.language
    │   ├── navigator.onLine (estado de conexión)
    │   ├── navigator.geolocation (getCurrentPosition, watchPosition)
    │   ├── navigator.mediaDevices (cámara, micrófono)
    │   ├── navigator.clipboard (portapapeles)
    │   ├── navigator.share (Web Share API)
    │   └── navigator.serviceWorker
    │
    ├── Location
    │   ├── location.href
    │   ├── location.protocol, host, pathname, search, hash
    │   ├── location.reload()
    │   ├── location.assign()
    │   ├── location.replace()
    │   └── URLSearchParams (manejar query string)
    │
    ├── History
    │   ├── history.back(), forward(), go()
    │   ├── history.pushState() (cambiar URL sin recargar)
    │   ├── history.replaceState()
    │   └── Evento popstate
    │
    ├── Console (ya visto)
    │
    ├── Timers (ya vistos)
    │
    ├── Fetch (ya visto)
    │
    ├── Web Storage (ya visto)
    │
    ├── Geolocation API
    │
    ├── Canvas API
    │   ├── Obtener contexto (getContext("2d"))
    │   ├── Dibujar rectángulos
    │   ├── Dibujar trazados (paths)
    │   ├── Estilos (fillStyle, strokeStyle)
    │   ├── Texto (fillText, strokeText)
    │   ├── Imágenes (drawImage)
    │   ├── Transformaciones (translate, rotate, scale)
    │   └── Animaciones con canvas
    │
    ├── Web Audio API (concepto)
    │
    ├── Intersection Observer
    │   ├── Observar cuando elementos entran/salen del viewport
    │   ├── Lazy loading de imágenes
    │   └── Carga infinita
    │
    ├── Mutation Observer
    │   ├── Observar cambios en el DOM
    │   └── Reaccionar a modificaciones
    │
    ├── Resize Observer
    │   └── Observar cambios de tamaño en elementos
    │
    ├── Performance API
    │   ├── performance.now()
    │   └── Medición de rendimiento
    │
    └── Web Components (concepto básico)
        ├── Custom Elements
        ├── Shadow DOM
        └── HTML Templates

```