---
title: Tree CSS
draft: true
---
# Tree

```tree
CSS/
│
├── 01 Fundamentos del Estilo/
│   ├── index.md
│   ├── 01 Sintaxis CSS/
│   │   ├── index.md
│   │   ├── 01 Selector, Propiedad y Valor.md
│   │   ├── 02 Declaración y Bloque.md
│   │   └── 03 Regla Completa.md
│   ├── 02 Formas de Incluir CSS/
│   │   ├── index.md
│   │   ├── 01 CSS Externo (link).md
│   │   ├── 02 CSS Interno (style).md
│   │   ├── 03 CSS en Línea (atributo style).md
│   │   └── 04 Importar (@import).md
│   ├── 03 Comentarios CSS.md
│   ├── 04 Selectores/
│   │   ├── index.md
│   │   ├── 01 Selectores Básicos/
│   │   │   ├── index.md
│   │   │   ├── 01 Selector de Tipo.md
│   │   │   ├── 02 Selector de Clase.md
│   │   │   ├── 03 Selector de ID.md
│   │   │   ├── 04 Selector Universal.md
│   │   │   └── 05 Agrupación de Selectores.md
│   │   ├── 02 Combinadores/
│   │   │   ├── index.md
│   │   │   ├── 01 Descendente.md
│   │   │   ├── 02 Hijo Directo (>).md
│   │   │   ├── 03 Hermano Adyacente (+).md
│   │   │   └── 04 Hermano General (~).md
│   │   └── 03 Selectores de Atributo.md            # tabla: [attr], ^=, $=, *=, ~=, |=
│   ├── 05 Colores/
│   │   ├── index.md
│   │   ├── 01 Palabras Clave de Color.md
│   │   ├── 02 Hexadecimal.md
│   │   ├── 03 RGB y RGBA.md
│   │   ├── 04 HSL y HSLA.md
│   │   ├── 05 HWB.md
│   │   ├── 06 LAB y LCH.md
│   │   └── 07 color-mix().md
│   ├── 06 Unidades de Medida/
│   │   ├── index.md
│   │   ├── 01 Unidades Absolutas (px, pt, cm).md
│   │   ├── 02 Relativas a Fuente (em, rem, ch, ex).md
│   │   ├── 03 Relativas al Viewport (vw, vh, vmin, vmax).md
│   │   ├── 04 Viewport Dinámico (svh, lvh, dvh).md
│   │   ├── 05 Porcentajes.md
│   │   ├── 06 calc().md
│   │   └── 07 min(), max(), clamp().md
│   ├── 07 Propiedades de Texto/
│   │   ├── index.md
│   │   ├── 01 color.md
│   │   ├── 02 font-family.md
│   │   ├── 03 font-size.md
│   │   ├── 04 font-weight.md
│   │   ├── 05 font-style.md
│   │   ├── 06 font-variant.md
│   │   ├── 07 line-height.md
│   │   ├── 08 text-align.md
│   │   ├── 09 text-decoration.md
│   │   ├── 10 text-transform.md
│   │   ├── 11 text-indent.md
│   │   ├── 12 letter-spacing y word-spacing.md
│   │   └── 13 white-space.md
│   └── 08 Herencia y Valores/
│       ├── index.md
│       ├── 01 Herencia Natural.md
│       └── 02 Valores Especiales (inherit, initial, unset, revert).md
│
├── 02 Modelo de Caja/
│   ├── index.md
│   ├── 01 Partes del Modelo de Caja.md
│   ├── 02 Dimensiones/
│   │   ├── index.md
│   │   ├── 01 width y height.md
│   │   ├── 02 min y max (width, height).md
│   │   ├── 03 aspect-ratio.md
│   │   └── 04 Tamaños de Contenido (fit-content, min-content, max-content).md
│   ├── 03 Padding.md
│   ├── 04 Border/
│   │   ├── index.md
│   │   ├── 01 border-width.md
│   │   ├── 02 border-style.md
│   │   ├── 03 border-color.md
│   │   ├── 04 Shorthand border.md
│   │   ├── 05 border-radius.md
│   │   └── 06 border-image.md
│   ├── 05 Margin.md                                 # incl. margin: auto
│   ├── 06 box-sizing.md
│   └── 07 Margin Collapse.md
│
├── 03 Tipografía Avanzada/
│   ├── index.md
│   ├── 01 Fuentes Web (@font-face)/
│   │   ├── index.md
│   │   ├── 01 src y format.md
│   │   ├── 02 font-display.md
│   │   └── 03 unicode-range.md
│   ├── 02 Propiedades Tipográficas Avanzadas/
│   │   ├── index.md
│   │   ├── 01 font-kerning.md
│   │   ├── 02 font-feature-settings.md
│   │   ├── 03 font-variation-settings.md
│   │   ├── 04 font-optical-sizing.md
│   │   ├── 05 font-stretch.md
│   │   ├── 06 text-rendering.md
│   │   └── 07 font-synthesis.md
│   ├── 03 Alineación Vertical (vertical-align).md
│   ├── 04 Decoración de Texto Avanzada/
│   │   ├── index.md
│   │   ├── 01 text-shadow.md
│   │   ├── 02 text-emphasis.md
│   │   ├── 03 text-stroke.md
│   │   └── 04 text-underline-offset y position.md
│   ├── 05 Manejo de Texto Multilínea/
│   │   ├── index.md
│   │   ├── 01 overflow-wrap y word-wrap.md
│   │   ├── 02 word-break.md
│   │   ├── 03 hyphens.md
│   │   ├── 04 line-clamp.md
│   │   └── 05 text-overflow.md
│   └── 06 Escritura y Dirección/
│       ├── index.md
│       ├── 01 writing-mode.md
│       ├── 02 direction.md
│       ├── 03 text-orientation.md
│       └── 04 unicode-bidi.md
│
├── 04 Fondos y Efectos Visuales/
│   ├── index.md
│   ├── 01 Propiedades de Fondo/
│   │   ├── index.md
│   │   ├── 01 background-color.md
│   │   ├── 02 background-image.md
│   │   ├── 03 background-repeat.md
│   │   ├── 04 background-position.md
│   │   ├── 05 background-size.md
│   │   ├── 06 background-attachment.md
│   │   ├── 07 background-clip y background-origin.md
│   │   ├── 08 Shorthand background.md
│   │   └── 09 Múltiples Fondos.md
│   ├── 02 Gradientes/
│   │   ├── index.md
│   │   ├── 01 linear-gradient.md
│   │   ├── 02 radial-gradient.md
│   │   ├── 03 conic-gradient.md
│   │   └── 04 Gradientes Repetidos.md
│   ├── 03 Sombras (box-shadow).md
│   ├── 04 Máscaras y Recortes/
│   │   ├── index.md
│   │   ├── 01 clip-path.md
│   │   ├── 02 mask.md
│   │   └── 03 Modos de Mezcla (mix-blend-mode, background-blend-mode).md
│   ├── 05 Filtros/
│   │   ├── index.md
│   │   ├── 01 filter.md                             # tabla: blur, brightness, contrast, grayscale...
│   │   └── 02 backdrop-filter.md
│   └── 06 Efectos de Borde/
│       ├── index.md
│       ├── 01 outline.md
│       └── 02 box-decoration-break.md
│
├── 05 Layout y Posicionamiento/
│   ├── index.md
│   ├── 01 Flujo Normal del Documento/
│   │   ├── index.md
│   │   ├── 01 display block.md
│   │   ├── 02 display inline.md
│   │   ├── 03 display inline-block.md
│   │   └── 04 display none.md
│   ├── 02 Posicionamiento (position)/
│   │   ├── index.md
│   │   ├── 01 static.md
│   │   ├── 02 relative.md
│   │   ├── 03 absolute.md
│   │   ├── 04 fixed.md
│   │   ├── 05 sticky.md
│   │   ├── 06 Desplazamiento (top, right, bottom, left).md
│   │   └── 07 z-index y Contexto de Apilamiento.md
│   ├── 03 Flexbox/
│   │   ├── index.md
│   │   ├── 01 Contenedor Flex (display flex).md
│   │   ├── 02 Dirección (flex-direction).md
│   │   ├── 03 Ajuste de Línea (flex-wrap).md
│   │   ├── 04 Eje Principal (justify-content).md
│   │   ├── 05 Eje Transversal (align-items, align-self).md
│   │   ├── 06 Líneas Múltiples (align-content).md
│   │   ├── 07 Espaciado (gap).md
│   │   ├── 08 order.md
│   │   ├── 09 flex-grow, flex-shrink, flex-basis.md
│   │   └── 10 Casos de Uso.md
│   ├── 04 CSS Grid/
│   │   ├── index.md
│   │   ├── 01 Contenedor Grid.md
│   │   ├── 02 Conceptos (pistas, líneas, celdas, áreas).md
│   │   ├── 03 grid-template-columns y rows.md
│   │   ├── 04 Unidad fr y repeat().md
│   │   ├── 05 minmax() y fit-content().md
│   │   ├── 06 subgrid.md
│   │   ├── 07 Ubicación por Líneas (grid-column, grid-row).md
│   │   ├── 08 Span.md
│   │   ├── 09 Áreas (grid-template-areas).md
│   │   ├── 10 Grid Implícito (auto-rows, auto-columns, auto-flow).md
│   │   ├── 11 Alineación en Grid (justify, align, place).md
│   │   └── 12 Patrones (auto-fit, auto-fill).md
│   ├── 05 Multicolumna/
│   │   ├── index.md
│   │   ├── 01 column-count y column-width.md
│   │   ├── 02 column-gap y column-rule.md
│   │   ├── 03 column-span.md
│   │   └── 04 break-inside.md
│   └── 06 Floats (legado)/
│       ├── index.md
│       ├── 01 float.md
│       ├── 02 clear.md
│       └── 03 Clearfix.md
│
├── 06 Diseño Responsivo/
│   ├── index.md
│   ├── 01 Viewport/
│   │   ├── index.md
│   │   ├── 01 meta viewport.md
│   │   ├── 02 viewport-fit.md
│   │   └── 03 interactive-widget.md
│   ├── 02 Media Queries/
│   │   ├── index.md
│   │   ├── 01 Sintaxis @media.md
│   │   ├── 02 Tipos de Medio.md
│   │   ├── 03 Operadores Lógicos.md
│   │   ├── 04 Dimensión (width, height, aspect-ratio).md
│   │   ├── 05 Interacción (hover, pointer).md
│   │   ├── 06 Preferencias (prefers-color-scheme, prefers-reduced-motion).md
│   │   └── 07 Breakpoints Comunes.md
│   ├── 03 Enfoques de Diseño/
│   │   ├── index.md
│   │   ├── 01 Mobile First.md
│   │   ├── 02 Desktop First.md
│   │   └── 03 Diseño Fluido.md
│   ├── 04 Imágenes Responsivas.md                  # -> delega a [[01 Imagen (img)]] (HTML)
│   ├── 05 Tipografía Responsiva/
│   │   ├── index.md
│   │   ├── 01 Unidades vw para Fuente.md
│   │   └── 02 clamp() para Tipografía Fluida.md
│   ├── 06 Layouts Responsivos/
│   │   ├── index.md
│   │   ├── 01 Flex-wrap para Componentes.md
│   │   ├── 02 Grid con auto-fit.md
│   │   ├── 03 Menús Hamburguesa.md
│   │   └── 04 Tablas Responsivas.md
│   └── 07 Container Queries/
│       ├── index.md
│       ├── 01 @container y container-type.md
│       └── 02 Unidades de Contenedor (cqw, cqh).md
│
├── 07 Animaciones y Transiciones/
│   ├── index.md
│   ├── 01 Transiciones (transition)/
│   │   ├── index.md
│   │   ├── 01 transition-property.md
│   │   ├── 02 transition-duration.md
│   │   ├── 03 transition-timing-function.md
│   │   ├── 04 transition-delay.md
│   │   └── 05 Propiedades Animables.md
│   ├── 02 Funciones de Tiempo/
│   │   ├── index.md
│   │   ├── 01 Curvas (ease, linear, ease-in-out).md
│   │   ├── 02 steps().md
│   │   └── 03 cubic-bezier().md
│   ├── 03 Transformaciones (transform)/
│   │   ├── index.md
│   │   ├── 01 Transformaciones 2D (translate, rotate, scale, skew).md
│   │   ├── 02 matrix().md
│   │   ├── 03 Transformaciones 3D (translateZ, rotateX, scale3d).md
│   │   ├── 04 perspective y perspective-origin.md
│   │   ├── 05 transform-origin.md
│   │   ├── 06 transform-style.md
│   │   └── 07 backface-visibility.md
│   ├── 04 Animaciones (@keyframes)/
│   │   ├── index.md
│   │   ├── 01 Definición de Fotogramas.md
│   │   ├── 02 animation-name y duration.md
│   │   ├── 03 animation-timing-function y delay.md
│   │   ├── 04 animation-iteration-count.md
│   │   ├── 05 animation-direction.md
│   │   ├── 06 animation-fill-mode.md
│   │   ├── 07 animation-play-state.md
│   │   └── 08 Múltiples Animaciones.md
│   ├── 05 Rendimiento en Animaciones/
│   │   ├── index.md
│   │   ├── 01 Layout, Paint y Composición.md
│   │   ├── 02 will-change.md
│   │   └── 03 Animar transform y opacity.md
│   └── 06 Animaciones de Scroll/
│       ├── index.md
│       ├── 01 scroll-timeline.md
│       └── 02 view-timeline.md
│
├── 08 Pseudoclases y Pseudoelementos/
│   ├── index.md
│   ├── 01 Pseudoclases de Interacción/
│   │   ├── index.md
│   │   ├── 01 hover.md
│   │   ├── 02 active.md
│   │   ├── 03 focus.md
│   │   ├── 04 focus-visible.md
│   │   ├── 05 focus-within.md
│   │   ├── 06 target.md
│   │   └── 07 visited.md
│   ├── 02 Pseudoclases de Estructura/
│   │   ├── index.md
│   │   ├── 01 first-child y last-child.md
│   │   ├── 02 first-of-type y last-of-type.md
│   │   ├── 03 nth-child().md
│   │   ├── 04 nth-of-type().md
│   │   ├── 05 only-child y only-of-type.md
│   │   ├── 06 empty.md
│   │   └── 07 root.md
│   ├── 03 Pseudoclases de Formulario/
│   │   ├── index.md
│   │   ├── 01 checked e indeterminate.md
│   │   ├── 02 disabled y enabled.md
│   │   ├── 03 read-only y read-write.md
│   │   ├── 04 required y optional.md
│   │   ├── 05 valid e invalid.md
│   │   ├── 06 in-range y out-of-range.md
│   │   └── 07 user-valid y user-invalid.md
│   ├── 04 Pseudoclases Funcionales/
│   │   ├── index.md
│   │   ├── 01 is().md
│   │   ├── 02 where().md
│   │   ├── 03 has().md
│   │   ├── 04 not().md
│   │   └── 05 lang().md
│   └── 05 Pseudoelementos/
│       ├── index.md
│       ├── 01 before y after.md
│       ├── 02 Propiedad content.md
│       ├── 03 first-letter.md
│       ├── 04 first-line.md
│       ├── 05 selection.md
│       ├── 06 placeholder.md
│       ├── 07 marker.md
│       ├── 08 backdrop.md
│       └── 09 file-selector-button.md
│
├── 09 Arquitectura y Metodologías/
│   ├── index.md
│   ├── 01 Especificidad/
│   │   ├── index.md
│   │   ├── 01 Cálculo de Especificidad.md
│   │   ├── 02 Especificidad de Pseudoclases y Pseudoelementos.md
│   │   └── 03 Especificidad de is() y where().md
│   ├── 02 Cascada/
│   │   ├── index.md
│   │   ├── 01 Orden de Origen.md
│   │   ├── 02 Importancia (!important).md
│   │   └── 03 Orígenes de Estilo (UA, usuario, autor).md
│   ├── 03 Capas (@layer)/
│   │   ├── index.md
│   │   ├── 01 Definición y Orden de Capas.md
│   │   └── 02 Importar a Capas.md
│   ├── 04 Metodologías de Nomenclatura/
│   │   ├── index.md
│   │   ├── 01 BEM.md
│   │   ├── 02 SMACSS.md
│   │   ├── 03 OOCSS.md
│   │   └── 04 Utility-First.md
│   └── 05 Organización de Archivos/
│       ├── index.md
│       ├── 01 Estructura Modular.md
│       └── 02 Patrón 7-1.md
│
└── 10 Variables CSS/
    ├── index.md
    ├── 01 Definición y Uso (--var, var()).md
    ├── 02 Valor por Defecto (fallback).md
    ├── 03 Ámbito y Herencia de Variables.md
    ├── 04 Variables en Media Queries.md
    ├── 05 Manipulación con JavaScript.md           # -> delega a [[Propiedad style]] (JS)
    └── 06 Temas Dinámicos.md
```

**Fuente:** destilado de `ARQUITECTURA.md` (atómico máximo, estilo Python: numerado por nivel, una nota por propiedad/concepto).
