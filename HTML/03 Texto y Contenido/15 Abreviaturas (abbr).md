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
> `<abbr>` marca una **abreviatura o acrónimo**. Con el atributo `title`, su forma completa aparece en un tooltip al pasar el cursor y queda disponible para las tecnologías de asistencia.

```html
<p>La <abbr title="Organización Mundial de la Salud">OMS</abbr>
   recomienda lavarse las manos.</p>
```

## El atributo title

| Con `title` | Sin `title` |
|-------------|-------------|
| Tooltip con la forma completa al pasar el ratón | Solo marca semántica de "esto es una abreviatura" |
| Disponible para lectores de pantalla | El usuario debe conocer ya el significado |

El `title` es el valor añadido de `<abbr>`: sin él, el elemento apenas aporta sobre el texto plano.

## La limitación del tooltip

> [!warning] El title no llega en táctil
> En pantallas táctiles (móviles, tablets) **no existe** el "pasar el cursor", así que el tooltip del `title` no se muestra. Por eso no se debe confiar únicamente en él para información esencial. La técnica robusta:

## Expandir la primera vez

La mejor práctica de accesibilidad combina la expansión en el texto con el `<abbr>` para las apariciones siguientes:

```html
<p>La Organización Mundial de la Salud (<abbr title="Organización Mundial de la Salud">OMS</abbr>)
   publicó el informe. La <abbr title="Organización Mundial de la Salud">OMS</abbr> lo confirmó.</p>
```

Así todos —incluido quien navega en móvil o con lector de pantalla— conocen el significado, y el `title` queda como apoyo, no como único canal.

## Combinar con dfn

Cuando la abreviatura es además el término que se está definiendo, se anida dentro de [[16 Definiciones (dfn) | `<dfn>`]]:

```html
<p>Una <dfn><abbr title="Application Programming Interface">API</abbr></dfn> es un
   conjunto de reglas para comunicar dos programas.</p>
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Escribe la forma completa la primera vez y abrevia después, con `<abbr title>` en cada aparición.
> - No dependas solo del `title`: en móvil no se ve.
> - Para acrónimos que defines, combina `<abbr>` dentro de `<dfn>`.

## Errores comunes

> [!warning] Trampas
> - **Información esencial solo en el `title`**: invisible en táctil; ponla también en el texto.
> - **`<abbr>` sin `title`** cuando el lector no conoce la sigla: no aporta nada.
> - **Repetir el `title` en cada palabra**: marca la sigla completa, no letra a letra.

## Notas relacionadas

- [[16 Definiciones (dfn)]] — para marcar el término que se está definiendo.
- [[03 Información Emergente (title)]] — el atributo global `title` y sus límites en táctil.
