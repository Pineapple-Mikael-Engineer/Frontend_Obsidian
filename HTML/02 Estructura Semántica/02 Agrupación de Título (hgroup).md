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
> `<hgroup>` agrupa un encabezado [[01 Encabezados Jerárquicos (h1-h6) | `<h1>`–`<h6>`]] con uno o
> varios párrafos `<p>` adyacentes que actúan como **subtítulo, eslogan o lema**. Mantiene unidos
> título y subtítulo sin que el subtítulo cree su propio nivel en el esquema.

```html
<hgroup>
  <h1>Frontend desde cero</h1>
  <p>Una guía práctica de HTML, CSS y JavaScript</p>
</hgroup>
```

## Qué cambió en HTML5 moderno

La definición de `<hgroup>` se revisó: hoy contiene **un único encabezado** más `<p>` opcionales,
no varios encabezados. El subtítulo va en un `<p>`, no en un `<h2>`, precisamente para que **no**
aparezca como sección aparte en el esquema del documento.

> [!warning] Soporte y necesidad limitados
> `<hgroup>` aporta poco valor semántico real: los lectores de pantalla apenas lo aprovechan. En la
> práctica, un `<p>` con una clase de subtítulo dentro del `<header>` suele bastar. Conocerlo es
> útil; usarlo es opcional.

> [!info] Alternativa común
> ```html
> <header>
>   <h1>Frontend desde cero</h1>
>   <p class="subtitulo">Guía práctica</p>
> </header>
> ```
> Logra el mismo efecto visual y semántico sin depender de `<hgroup>`.

## Notas relacionadas

- [[01 Encabezados Jerárquicos (h1-h6)]] — el encabezado que agrupa.
- [[08 Cabecera de Sección (header)]] — el contenedor habitual de un título + subtítulo.
