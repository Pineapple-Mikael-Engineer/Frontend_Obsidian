---
title: Tree HTML
draft: true
---
# Tree

```tree
HTML/
│
├── 01 Estructura Documento/
│   ├── index.md
│   ├── 01 Declaración DOCTYPE.md
│   ├── 02 Elemento Raíz (html).md                  # lang, dir
│   ├── 03 Cabecera (head)/
│   │   ├── index.md
│   │   ├── 01 Codificación de Caracteres (meta charset).md
│   │   ├── 02 Viewport (meta viewport).md
│   │   ├── 03 Título del Documento (title).md
│   │   ├── 04 Descripción (meta description).md
│   │   ├── 05 Palabras Clave (meta keywords).md
│   │   ├── 06 Autor (meta author).md
│   │   ├── 07 Enlace a CSS (link).md
│   │   ├── 08 Estilos Internos (style).md
│   │   ├── 09 Scripts (script).md
│   │   ├── 10 Favicon (link rel icon).md
│   │   └── 11 Enlace Canónico (link rel canonical).md
│   └── 04 Cuerpo (body).md
│
├── 02 Estructura Semántica/
│   ├── index.md
│   ├── 01 Encabezados Jerárquicos (h1-h6).md
│   ├── 02 Agrupación de Título (hgroup).md
│   ├── 03 Navegación (nav).md
│   ├── 04 Contenido Principal (main).md
│   ├── 05 Secciones (section).md
│   ├── 06 Artículos (article).md
│   ├── 07 Contenido Complementario (aside).md
│   ├── 08 Cabecera de Sección (header).md
│   ├── 09 Pie de Sección (footer).md
│   ├── 10 Dirección (address).md
│   └── 11 Figura (figure, figcaption).md
│
├── 03 Texto y Contenido/
│   ├── index.md
│   ├── 01 Párrafos (p).md
│   ├── 02 Saltos de Línea (br).md
│   ├── 03 Línea Horizontal (hr).md
│   ├── 04 Énfasis Fuerte (strong).md
│   ├── 05 Énfasis (em).md
│   ├── 06 Negrita sin Énfasis (b).md
│   ├── 07 Cursiva sin Énfasis (i).md
│   ├── 08 Texto Pequeño (small).md
│   ├── 09 Subrayado (u).md
│   ├── 10 Tachado (s, del).md
│   ├── 11 Texto Insertado (ins).md
│   ├── 12 Texto Resaltado (mark).md
│   ├── 13 Citas en Bloque (blockquote).md
│   ├── 14 Citas en Línea (q).md
│   ├── 15 Abreviaturas (abbr).md
│   ├── 16 Definiciones (dfn).md
│   ├── 17 Código (code).md
│   ├── 18 Código Preformateado (pre).md
│   ├── 19 Variable (var).md
│   ├── 20 Salida de Programa (samp).md
│   ├── 21 Entrada de Teclado (kbd).md
│   ├── 22 Superíndice y Subíndice (sup, sub).md
│   ├── 23 Tiempo (time).md
│   ├── 24 Texto Bidireccional (bdo, bdi).md
│   └── 25 Ruptura de Palabra (wbr).md
│
├── 04 Listas/
│   ├── index.md
│   ├── 01 Listas Ordenadas (ol).md                 # type, start, reversed
│   ├── 02 Listas No Ordenadas (ul).md
│   ├── 03 Elementos de Lista (li).md
│   ├── 04 Listas de Definición (dl, dt, dd).md
│   └── 05 Listas Anidadas.md
│
├── 05 Enlaces y Navegación/
│   ├── index.md
│   ├── 01 Enlaces (a).md                           # tabla: href, target, rel, download, hreflang, type
│   ├── 02 Anclas Internas (id).md
│   ├── 03 Enlaces a Correo (mailto).md
│   └── 04 Enlaces Telefónicos (tel).md
│
├── 06 Tablas/
│   ├── index.md
│   ├── 01 Contenedor de Tabla (table).md
│   ├── 02 Fila de Tabla (tr).md
│   ├── 03 Celda de Encabezado (th).md              # scope, abbr
│   ├── 04 Celda de Datos (td).md
│   ├── 05 Fusión de Celdas (colspan, rowspan).md
│   ├── 06 Agrupación de Columnas (colgroup, col).md
│   ├── 07 Secciones (thead, tbody, tfoot).md
│   └── 08 Título de Tabla (caption).md
│
├── 07 Formularios/
│   ├── index.md
│   ├── 01 Contenedor de Formulario (form).md       # tabla: action, method, enctype, novalidate, autocomplete
│   ├── 02 Campos de Entrada (input)/
│   │   ├── index.md
│   │   ├── 01 Atributos Comunes de input.md        # type, name, value, placeholder, required, disabled, readonly...
│   │   ├── 02 Validación Específica por Tipo.md
│   │   └── 03 Tipos de input/
│   │       ├── index.md
│   │       ├── 01 input text.md
│   │       ├── 02 input password.md
│   │       ├── 03 input email.md
│   │       ├── 04 input number.md
│   │       ├── 05 input tel.md
│   │       ├── 06 input url.md
│   │       ├── 07 input search.md
│   │       ├── 08 input hidden.md
│   │       ├── 09 input checkbox.md
│   │       ├── 10 input radio.md
│   │       ├── 11 input file.md
│   │       ├── 12 input date.md
│   │       ├── 13 input datetime-local.md
│   │       ├── 14 input month.md
│   │       ├── 15 input week.md
│   │       ├── 16 input time.md
│   │       ├── 17 input color.md
│   │       ├── 18 input range.md
│   │       ├── 19 input image.md
│   │       ├── 20 input submit.md
│   │       ├── 21 input reset.md
│   │       └── 22 input button.md
│   ├── 03 Área de Texto (textarea).md              # rows, cols, wrap, maxlength
│   ├── 04 Listas de Selección (select)/
│   │   ├── index.md
│   │   ├── 01 Selección (select).md                # multiple, size
│   │   ├── 02 Opciones (option).md                 # value, selected, disabled
│   │   └── 03 Agrupación de Opciones (optgroup).md
│   ├── 05 Lista de Datos (datalist).md
│   ├── 06 Botones (button).md                      # submit, reset, button
│   ├── 07 Etiquetas y Agrupación/
│   │   ├── index.md
│   │   ├── 01 Etiqueta de Campo (label).md         # for, anidamiento
│   │   ├── 02 Agrupación de Campos (fieldset).md
│   │   └── 03 Leyenda de Grupo (legend).md
│   ├── 08 Elementos de Salida/
│   │   ├── index.md
│   │   ├── 01 Resultado de Cálculo (output).md
│   │   ├── 02 Medidor (meter).md
│   │   └── 03 Barra de Progreso (progress).md
│   └── 09 Validación de Formularios/
│       ├── index.md
│       ├── 01 Validación Nativa HTML5.md
│       ├── 02 Atributo pattern.md
│       ├── 03 Restricciones (required, min, max, step, maxlength).md
│       ├── 04 Pseudoclases de Validación.md        # -> delega a [[Pseudoclases de Formulario]] (CSS)
│       └── 05 Constraint Validation API.md          # setCustomValidity -> delega a JS
│
├── 08 Multimedia e Incrustación/
│   ├── index.md
│   ├── 01 Imágenes/
│   │   ├── index.md
│   │   ├── 01 Imagen (img).md                      # tabla: src, alt, width/height, loading, decoding, srcset, sizes
│   │   ├── 02 Imágenes Responsivas (picture, source).md
│   │   ├── 03 Mapa de Imagen (map, area).md
│   │   └── 04 Formatos de Imagen.md                # jpg, png, gif, svg, webp, avif
│   ├── 02 Video y Audio/
│   │   ├── index.md
│   │   ├── 01 Video (video).md                     # controls, autoplay, muted, loop, poster, preload, playsinline
│   │   ├── 02 Audio (audio).md
│   │   ├── 03 Fuentes (source).md
│   │   └── 04 Pistas de Texto (track).md           # kind, srclang, label, default
│   ├── 03 Incrustación/
│   │   ├── index.md
│   │   ├── 01 Marco en Línea (iframe).md           # src, srcdoc, sandbox, allow, loading, referrerpolicy
│   │   ├── 02 Objeto Incrustado (object).md
│   │   └── 03 Incrustación Genérica (embed).md
│   └── 04 Gráficos/
│       ├── index.md
│       ├── 01 Lienzo (canvas).md                   # getContext. Dibujo -> delega a [[Canvas API]] (JS)
│       └── 02 Gráficos Vectoriales (svg).md        # circle, rect, path, g; inline vs imagen
│
├── 09 Metadatos y SEO/
│   ├── index.md
│   ├── 01 Metadatos Estándar/
│   │   ├── index.md
│   │   ├── 01 Idioma (html lang).md
│   │   ├── 02 Robots (meta robots).md
│   │   ├── 03 Verificación de Sitio.md
│   │   └── 04 Refresh y Redirección (http-equiv).md
│   ├── 02 Open Graph/
│   │   ├── index.md
│   │   ├── 01 Propiedades Básicas (og title, type, url).md
│   │   ├── 02 Imagen (og image).md
│   │   └── 03 Sitio y Locale (og site_name, locale).md
│   ├── 03 Twitter Cards/
│   │   ├── index.md
│   │   ├── 01 Tipo de Tarjeta (twitter card).md
│   │   ├── 02 Contenido (title, description, image).md
│   │   └── 03 Atribución (site, creator).md
│   └── 04 Datos Estructurados (JSON-LD)/
│       ├── index.md
│       ├── 01 Schema.org Básico.md
│       ├── 02 Article.md
│       ├── 03 Person y Organization.md
│       ├── 04 Product.md
│       ├── 05 Event.md
│       ├── 06 Recipe.md
│       ├── 07 FAQPage.md
│       ├── 08 BreadcrumbList.md
│       └── 09 LocalBusiness.md
│
├── 10 Accesibilidad (A11Y)/
│   ├── index.md
│   ├── 01 HTML Semántico como Base.md
│   ├── 02 ARIA/
│   │   ├── index.md
│   │   ├── 01 Roles de Landmark.md
│   │   ├── 02 Roles de Estructura.md
│   │   ├── 03 Roles de Widget.md
│   │   ├── 04 Propiedades ARIA (label, labelledby, describedby).md
│   │   ├── 05 Estados ARIA (expanded, hidden, checked, selected).md
│   │   └── 06 Regiones Vivas (aria-live, aria-atomic, aria-relevant).md
│   ├── 03 Navegación por Teclado/
│   │   ├── index.md
│   │   ├── 01 tabindex.md
│   │   ├── 02 Gestión de Foco (focus, blur).md
│   │   ├── 03 Skip Links.md
│   │   └── 04 Trampas de Foco (focus trapping).md
│   ├── 04 Textos Alternativos/
│   │   ├── index.md
│   │   ├── 01 alt en Imágenes.md
│   │   ├── 02 Texto para Iconos.md
│   │   └── 03 Contenido Oculto Visualmente (sr-only).md
│   ├── 05 Formularios Accesibles.md
│   └── 06 Multimedia Accesible/
│       ├── index.md
│       ├── 01 Subtítulos (captions).md
│       ├── 02 Transcripciones.md
│       └── 03 Audiodescripción.md
│
└── 11 Atributos Globales/
    ├── index.md
    ├── 01 Identificación (id, class).md
    ├── 02 Estilo en Línea (style).md
    ├── 03 Información Emergente (title).md
    ├── 04 Idioma y Dirección (lang, dir).md
    ├── 05 Ocultar (hidden).md
    ├── 06 Tabulación (tabindex).md
    ├── 07 Tecla de Acceso (accesskey).md
    ├── 08 Arrastrable (draggable).md
    ├── 09 Editable (contenteditable).md
    ├── 10 Corrección Ortográfica (spellcheck).md
    ├── 11 Traducción (translate).md
    ├── 12 Atributos de Datos (data-*).md           # -> delega a [[Modificar Atributos]] (JS)
    ├── 13 Inactivo (inert).md
    └── 14 Web Components (slot, part, exportparts).md
```

**Fuente:** destilado de `ARQUITECTURA.md` (atómico máximo, estilo Python: numerado por nivel, una nota por elemento).
