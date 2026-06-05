---
title: <body> — Contenido visible del documento
aliases:
  - body element
tags:
  - html
  - api/elemento
  - estructura
elemento: body
categoria: seccionado
rol_implicito: ninguno
vacio: false
draft: false
---

# Cuerpo (body)

> [!definicion]
> `<body>` contiene **todo el contenido que el usuario ve**: texto, imágenes, formularios, enlaces.
> Es el segundo y último hijo de [[02 Elemento Raíz (html) | `<html>`]], después de
> [[03 Cabecera (head)/index | `<head>`]]. Solo puede haber **uno** por documento.

```html
<body>
  <header>…</header>
  <main>…</main>
  <footer>…</footer>
</body>
```

Lo que está fuera del `<body>` no se renderiza como contenido (vive en el `<head>` como metadato).
El navegador es tolerante: si se escribe contenido sin `<body>` explícito, el parser lo crea
implícitamente y mueve el contenido dentro.

## Eventos asociados

El `<body>` es el destino histórico de varios eventos de la ventana, aunque hoy se prefiere
registrarlos sobre `window` desde JavaScript:

| Evento | Se dispara cuando |
|--------|-------------------|
| `load` | Toda la página y sus recursos terminaron de cargar |
| `DOMContentLoaded` | El DOM está listo (sin esperar imágenes) — se escucha en `document` |
| `beforeunload` | El usuario va a abandonar la página |

> [!warning] Sin estilos ni scripts inline
> El `<body>` no es el lugar para `<style>` ni `style="…"` inline (presentación → CSS) ni para
> `onclick="…"` (comportamiento → JS). La estructura va en HTML; el resto se delega para mantener
> separadas las tres capas.

> [!info] Estructura recomendada dentro del body
> Un `<body>` bien formado se organiza con landmarks semánticos —
> [[08 Cabecera de Sección (header) | `<header>`]], [[04 Contenido Principal (main) | `<main>`]],
> [[09 Pie de Sección (footer) | `<footer>`]]— no con `<div>` genéricos. Eso da navegación por
> regiones a los lectores de pantalla sin esfuerzo extra.

## Notas relacionadas

- [[03 Cabecera (head)/index]] — su contraparte invisible.
- [[02 Estructura Semántica/index]] — cómo organizar el contenido del body.
- [[04 Contenido Principal (main)]] — el contenido central dentro del body.
