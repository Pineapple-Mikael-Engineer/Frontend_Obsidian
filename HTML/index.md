---
title: HTML — Documentación de referencia
aliases:
  - HTML
  - HyperText Markup Language
tags:
  - html
  - api/concepto
draft: false
order: 2
---

# HTML

> [!definicion]
> HTML (HyperText Markup Language) es el lenguaje de marcado que estructura el contenido de la web. Cada elemento aporta **significado semántico** que navegadores, motores de búsqueda y tecnologías asistivas interpretan. HTML no da estilos ni comportamiento: solo define **qué es** cada pieza de contenido, no cómo se ve ni qué hace.

Un documento HTML bien formado es un árbol de elementos anidados donde cada etiqueta comunica intención: `<article>` dice "esto es un artículo independiente", `<nav>` dice "esto es navegación", `<h1>` dice "esto es el título principal". La diferencia entre HTML semántico y un *div-soup* no es visual, es la calidad de información que recibe todo agente que procesa el documento.

```html
<!doctype html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Ejemplo semántico</title>
  </head>
  <body>
    <header>
      <nav aria-label="Principal">...</nav>
    </header>
    <main>
      <article>
        <h1>Título del artículo</h1>
        <p>Contenido con <strong>énfasis fuerte</strong> y <em>énfasis leve</em>.</p>
      </article>
    </main>
    <footer>...</footer>
  </body>
</html>
```

## Contenido de la sección

| # | Nombre | Cubre |
|---|---|---|
| 01 | Estructura Documento | Anatomía del HTML: `<!doctype>`, `<html>`, `<head>`, `<body>`; flujo de parsing; árbol DOM |
| 02 | Estructura Semántica | Elementos de sección: `<header>`, `<main>`, `<footer>`, `<article>`, `<section>`, `<aside>`, `<nav>` |
| 03 | Texto y Contenido | Encabezados, párrafos, énfasis, citas, código, abreviaciones y el resto de contenido en línea |
| 04 | Listas | `<ul>`, `<ol>`, `<dl>` y cuándo usar cada tipo; listas anidadas; role semántico |
| 05 | Enlaces y Navegación | `<a>` y sus atributos; URLs absolutas vs. relativas; atributos de seguridad; `<link>` en head |
| 06 | Tablas | `<table>` con `<thead>`, `<tbody>`, `<tfoot>`, `<caption>`; atributos de fusión; tablas accesibles |
| 07 | Formularios | `<form>`, `<input>` y sus tipos, `<select>`, `<textarea>`, `<button>`; validación nativa; asociación label-control |
| 08 | Multimedia e Incrustación | `<img>`, `<picture>`, `<video>`, `<audio>`, `<iframe>`, `<embed>`, `<object>`; formatos y atributos clave |
| 09 | Metadatos y SEO | Etiquetas `<meta>`, Open Graph, Twitter Cards, `<link rel>`, datos estructurados y head óptimo |
| 10 | Accesibilidad (A11Y) | ARIA roles y atributos, orden de foco, texto alternativo, contraste, semántica como base de accesibilidad |
| 11 | Atributos Globales | `id`, `class`, `data-*`, `hidden`, `tabindex`, `lang`, `contenteditable`, `draggable` y el resto de atributos universales |

## Arco de aprendizaje

Las secciones siguen una progresión deliberada. Las dos primeras (01-02) cubren la **anatomía del documento**: cómo se forma el árbol HTML y qué elementos de sección dan estructura al layout de alto nivel. Sin esto, el resto no tiene dónde anclarse.

Las secciones 03-06 cubren los **bloques de contenido**: texto, listas, enlaces y tablas son los cuatro tipos de contenido que componen la mayor parte de cualquier página. Se leen en orden porque los enlaces dependen de entender texto, y las tablas son la pieza más compleja del grupo.

La sección 07 (Formularios) es un capítulo propio: los formularios son la **interfaz de entrada del usuario**, el único mecanismo nativo de HTML para recoger datos, y tienen suficiente complejidad para tratarse aislados.

La sección 08 (Multimedia) cierra el catálogo de elementos con los recursos incrustados: imágenes, vídeo, audio e iframes.

Las tres últimas secciones (09-11) son **capas transversales** que aplican sobre todo lo anterior. SEO y metadatos (09) afectan al head y a cómo interpretan el documento los buscadores. Accesibilidad (10) atraviesa cada elemento del árbol. Atributos globales (11) son el vocabulario de atributos disponible en cualquier etiqueta.

## Notas relacionadas

- [[01 Estructura Documento/index|Estructura Documento]]
- [[02 Estructura Semántica/index|Estructura Semántica]]
- [[03 Texto y Contenido/index|Texto y Contenido]]
- [[04 Listas/index|Listas]]
- [[05 Enlaces y Navegación/index|Enlaces y Navegación]]
- [[06 Tablas/index|Tablas]]
- [[07 Formularios/index|Formularios]]
- [[08 Multimedia e Incrustación/index|Multimedia e Incrustación]]
- [[09 Metadatos y SEO/index|Metadatos y SEO]]
- [[10 Accesibilidad (A11Y)/index|Accesibilidad (A11Y)]]
- [[11 Atributos Globales/index|Atributos Globales]]
