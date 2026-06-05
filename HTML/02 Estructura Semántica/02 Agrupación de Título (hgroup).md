---
title: <hgroup> — Agrupar un encabezado con su subtítulo
aliases:
  - hgroup
tags:
  - html
  - api/elemento
  - semantica
elemento: hgroup
categoria: flujo
rol_implicito: ninguno
vacio: false
draft: false
---

# Agrupación de Título (hgroup)

> [!definicion]
> `<hgroup>` agrupa un encabezado `<h1>`–`<h6>` con uno o varios párrafos `<p>` adyacentes que actúan
> como **subtítulo, eslogan o lema**. Mantiene unidos título y subtítulo como una unidad, sin que el
> subtítulo cree su propio nivel en el esquema del documento.

```html
<hgroup>
  <h1>Frontend desde cero</h1>
  <p>Una guía práctica de HTML, CSS y JavaScript</p>
</hgroup>
```

## El problema que intenta resolver

Un subtítulo no es un título de menor nivel. Si se marca con un encabezado real, contamina el
esquema:

```html
<!-- ❌ El subtítulo crea una sección h2 falsa en el esquema -->
<h1>Frontend desde cero</h1>
<h2>Una guía práctica</h2>

<!-- ✅ El subtítulo es un p; no aparece en el esquema -->
<hgroup>
  <h1>Frontend desde cero</h1>
  <p>Una guía práctica</p>
</hgroup>
```

El objetivo de `<hgroup>` es precisamente que el subtítulo **no** figure como un nivel del outline,
sino como un acompañante del título.

## Qué cambió en su definición

La especificación de `<hgroup>` se revisó con los años. La versión vigente contiene **un único
encabezado** más `<p>` opcionales (antes/después), no varios encabezados como en la propuesta
original. Por eso el subtítulo va en un `<p>`, no en un segundo `<h2>`.

## Alternativa sin hgroup

En la práctica, el mismo resultado semántico y visual se logra con un `<header>` o un contenedor y
una clase, sin depender de `<hgroup>`:

```html
<header>
  <h1>Frontend desde cero</h1>
  <p class="subtitulo">Guía práctica de HTML, CSS y JavaScript</p>
</header>
```

```css
.subtitulo { font-size: 1.25rem; color: #777; font-weight: 400; }
```

## Buenas prácticas

> [!tip] Recomendación pragmática
> `<hgroup>` aporta poco valor semántico real: las tecnologías de asistencia apenas lo aprovechan, y
> su historia accidentada genera dudas de soporte. No está mal usarlo, pero **un `<p>` de subtítulo
> dentro de un `<header>`** suele ser la opción más simple y robusta. Conocer `<hgroup>` es útil;
> usarlo es opcional.

## Errores comunes

> [!warning] No metas un h2 como subtítulo
> Marcar el subtítulo con `<h2>` (dentro o fuera de `<hgroup>`) lo introduce en el esquema como si
> fuera una sección, rompiendo la jerarquía. El subtítulo es texto acompañante: va en un `<p>`.

## Notas relacionadas

- [[01 Encabezados Jerárquicos (h1-h6)]] — el encabezado que `<hgroup>` agrupa y el esquema que protege.
- [[08 Cabecera de Sección (header)]] — el contenedor habitual de un título con su subtítulo.
