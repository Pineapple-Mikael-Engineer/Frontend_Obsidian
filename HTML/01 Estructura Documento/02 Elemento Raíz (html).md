---
title: <html> — Elemento raíz del documento
aliases:
  - html element
  - root element
tags:
  - html
  - api/elemento
  - estructura
elemento: html
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
---

# Elemento Raíz (html)

> [!definicion]
> `<html>` es el **nodo raíz** del árbol DOM: todo el documento cuelga de él. Contiene exactamente
> dos hijos en orden: [[03 Cabecera (head)/index | `<head>`]] y luego
> [[04 Cuerpo (body) | `<body>`]].

```html
<!DOCTYPE html>
<html lang="es" dir="ltr">
  <head> … </head>
  <body> … </body>
</html>
```

## Atributos clave

| Atributo | Valores | Descripción |
|----------|---------|-------------|
| `lang` | código BCP 47 (`es`, `en`, `es-MX`) | Idioma principal del documento. **Crítico**. |
| `dir` | `ltr` · `rtl` · `auto` | Dirección base del texto (izq→der, der→izq). |

El atributo `lang` no es decorativo: gobierna la pronunciación de los lectores de pantalla, la
selección de tipografías y reglas de guionado, la traducción automática y el corrector ortográfico.
Una página en español con `lang="en"` se lee con acento inglés por el lector de pantalla.

> [!info] Accesibilidad
> `lang` en `<html>` es uno de los criterios de WCAG (3.1.1, nivel A). Es el primer ajuste de
> accesibilidad de cualquier página y el más barato: un solo atributo. Para fragmentos en otro
> idioma, se sobrescribe localmente con `lang` en el elemento concreto (atributo global).

> [!warning] No confundir raíz del DOM con raíz de estilo
> En CSS, `:root` apunta a este `<html>`, pero tiene **mayor especificidad** que el selector de
> tipo `html`. Es el lugar canónico para declarar variables CSS (custom properties) globales.

## Notas relacionadas

- [[01 Declaración DOCTYPE]] — la línea que lo precede.
- [[03 Cabecera (head)/index]] — su primer hijo.
- [[04 Cuerpo (body)]] — su segundo hijo.
- [[04 Idioma y Dirección (lang, dir)]] — `lang`/`dir` como atributos globales.
