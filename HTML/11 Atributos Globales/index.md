---
title: Atributos Globales — Atributos comunes a todos los elementos
aliases:
  - global attributes
  - atributos globales
tags:
  - html
  - api/concepto
  - atributos
draft: false
order: 11
---

# Atributos Globales

> [!definicion]
> Los **atributos globales** se pueden aplicar a **cualquier** elemento HTML, no solo a unos pocos: identificación (`id`, `class`), idioma (`lang`), visibilidad (`hidden`), foco (`tabindex`), datos a medida (`data-*`), y más. Son las herramientas transversales que se combinan con todos los elementos de las secciones anteriores.

```html
<div id="caja" class="destacado" lang="en" hidden data-estado="activo">…</div>
```

## Mapa de la sección

| Atributo | Para qué |
|----------|----------|
| `id`, `class` | Identificar y clasificar elementos |
| `style` | Estilo en línea (a evitar) |
| `title` | Información emergente (tooltip) |
| `lang`, `dir` | Idioma y dirección del texto |
| `hidden` | Ocultar un elemento |
| `tabindex` | Control del foco por teclado |
| `accesskey` | Atajo de teclado |
| `draggable` | Permitir arrastrar |
| `contenteditable` | Hacer editable el contenido |
| `spellcheck` | Corrección ortográfica |
| `translate` | Permitir o no la traducción |
| `data-*` | Datos personalizados para JavaScript |
| `inert` | Desactivar una rama del DOM |
| `slot`, `part` | Web Components |

Cada uno tiene su nota: [[01 Identificación (id, class)]], [[02 Estilo en Línea (style)]], [[03 Información Emergente (title)]], [[04 Idioma y Dirección (lang, dir)]], [[05 Ocultar (hidden)]], [[06 Tabulación (tabindex)]], [[07 Tecla de Acceso (accesskey)]], [[08 Arrastrable (draggable)]], [[09 Editable (contenteditable)]], [[10 Corrección Ortográfica (spellcheck)]], [[11 Traducción (translate)]], [[12 Atributos de Datos (data-*)]], [[13 Inactivo (inert)]] y [[14 Web Components (slot, part, exportparts)]].

## Los dos más usados con diferencia

> [!info] id y class, el pan de cada día
> De todos los atributos globales, [[01 Identificación (id, class) | `id` y `class`]] son los que aparecen en casi cada línea de HTML: son el puente con CSS (selectores) y con JavaScript (`getElementById`, `querySelector`). El resto son especializados; estos dos son universales.

## La separación de capas, también aquí

Algunos atributos globales **existen pero conviene evitarlos** porque mezclan capas: [[02 Estilo en Línea (style) | `style`]] mete CSS en el HTML, y los manejadores `on*` (`onclick`) meten JS. La buena práctica mantiene el HTML para la estructura, delegando estilo a CSS (vía `class`) y comportamiento a JS (vía `id`/`data-*`).

## Notas relacionadas

- [[01 Identificación (id, class)]] — los atributos más fundamentales.
- [[12 Atributos de Datos (data-*)]] — el puente de datos hacia JavaScript.
