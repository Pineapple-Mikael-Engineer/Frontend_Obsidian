---
title: <abbr> — Abreviatura o acrónimo
aliases:
  - abbr
  - abbreviation
tags:
  - html
  - api/elemento
  - semantica
elemento: abbr
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
---

# Abreviaturas (abbr)

> [!definicion]
> `<abbr>` marca una **abreviatura o acrónimo**. Con el atributo `title`, su forma completa aparece
> en un tooltip al pasar el cursor y la exponen las tecnologías de asistencia.

```html
<p>La <abbr title="Organización Mundial de la Salud">OMS</abbr>
   recomienda lavarse las manos.</p>
```

## El atributo `title`

| Con `title` | Sin `title` |
|-------------|-------------|
| Tooltip con la forma completa | Solo marca semántica de "es una abreviatura" |
| Accesible a lectores de pantalla | El usuario debe conocer el significado |

> [!tip] Expandir la primera vez
> La mejor práctica de accesibilidad es **escribir la forma completa la primera vez** y abreviar
> después: "Organización Mundial de la Salud (OMS)… la `<abbr>OMS</abbr>`". El `title` es un apoyo,
> no el canal principal, porque en táctil no hay hover.

> [!warning] El tooltip no llega en móvil
> En pantallas táctiles no existe el "pasar el cursor", así que el `title` no se muestra. No confiar
> únicamente en él para información esencial: expandir en el texto.

## Notas relacionadas

- [[16 Definiciones (dfn)]] — para marcar el término que se está definiendo.
- [[03 Información Emergente (title)]] — el atributo global `title` y sus límites.
