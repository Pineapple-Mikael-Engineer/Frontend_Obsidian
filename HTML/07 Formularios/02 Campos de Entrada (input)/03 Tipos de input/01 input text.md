---
title: input type="text" — Texto libre de una línea
aliases:
  - input text
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: textbox
vacio: true
draft: false
order: 1
---

# input text

> [!definicion]
> `<input type="text">` es el campo de **texto libre de una sola línea**. Es el tipo por defecto (si se omite `type`) y el más genérico: acepta cualquier carácter sin validación de formato. Para texto multilínea se usa [[03 Área de Texto (textarea) | `<textarea>`]].

```html
<label for="nombre">Nombre completo</label>
<input type="text" id="nombre" name="nombre" maxlength="60" autocomplete="name" />
```

## Atributos más usados

| Atributo | Efecto |
|----------|--------|
| `maxlength` / `minlength` | Límite de caracteres |
| `pattern` | Expresión regular que el valor debe cumplir |
| `placeholder` | Texto de ejemplo |
| `size` | Ancho visible en caracteres (preferible controlar con CSS) |
| `autocomplete` | Sugerencias del navegador |
| `spellcheck` | Corrección ortográfica (`true`/`false`) |

## Cuándo text y cuándo un tipo específico

`text` es el comodín, pero casi siempre hay un tipo mejor: un correo es [[03 input email]], una URL es [[06 input url]], un número es [[04 input number]]. Reservar `text` para datos sin formato definido (nombre, ciudad, título).

## Validación con pattern

Como `text` no valida formato, se le añade con [[02 Atributo pattern | `pattern`]]:

```html
<!-- Solo letras y espacios -->
<input type="text" pattern="[A-Za-zÁÉÍÓÚñ ]+" title="Solo letras" />
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `text` solo para datos sin formato específico; si hay un tipo dedicado, úsalo.
> - Controla el ancho con CSS (`width`), no con `size`.
> - Añade `autocomplete` y `maxlength` cuando apliquen.

## Errores comunes

> [!warning] Trampas
> - **Usar `text` para correos/números/URLs**: pierdes validación y el teclado móvil adecuado.
> - **Confiar en `maxlength` como validación de seguridad**: es saltable; revalida en servidor.

## Notas relacionadas

- [[01 Atributos Comunes de input]] — los atributos compartidos.
- [[03 Área de Texto (textarea)]] — para texto de varias líneas.
- [[02 Atributo pattern]] — validar el formato del texto.
