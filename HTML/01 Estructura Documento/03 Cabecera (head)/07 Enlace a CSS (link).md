---
title: <link rel="stylesheet"> — Enlace a hoja de estilos externa
aliases:
  - link element
  - stylesheet link
tags:
  - html
  - api/elemento
  - metadatos
elemento: link
categoria: metadatos
rol_implicito: ninguno
vacio: true
draft: false
---

# Enlace a CSS (link)

> [!definicion]
> `<link rel="stylesheet" href="…">` vincula una **hoja de estilos externa** al documento. Es la
> forma recomendada de aplicar CSS: separa presentación de estructura y permite cachear el `.css`
> entre páginas. El elemento `<link>` es void (sin cierre).

```html
<head>
  <link rel="stylesheet" href="/css/estilos.css" />
</head>
```

## Atributos

| Atributo | Valores | Descripción |
|----------|---------|-------------|
| `rel` | `stylesheet`, `icon`, `canonical`, `preload`… | **Relación** del recurso enlazado |
| `href` | URL | Ruta a la hoja de estilos |
| `media` | `screen`, `print`, `(max-width: 600px)` | Condición para aplicar la hoja |
| `crossorigin` | `anonymous`, `use-credentials` | Política CORS para recursos de otro origen |

El mismo elemento `<link>` sirve para muchos propósitos según `rel`: aquí estilos, pero también
[[10 Favicon (link rel icon) | favicon]] y [[11 Enlace Canónico (link rel canonical) | canónico]].

> [!tip] Carga condicional con media
> `<link rel="stylesheet" href="print.css" media="print">` solo aplica al imprimir. El navegador
> descarga la hoja con baja prioridad si la condición no se cumple, mejorando el render inicial.

> [!warning] El CSS bloquea el render
> Las hojas de estilo en el `<head>` son **render-blocking**: el navegador no pinta hasta tenerlas.
> Eso evita el "flash" de contenido sin estilo (FOUC), pero un CSS pesado retrasa el primer pintado.
> Por eso el CSS va en el `<head>` y el JS pesado al final o con `defer`.

## Notas relacionadas

- [[08 Estilos Internos (style)]] — CSS embebido, alternativa para casos puntuales.
- [[09 Scripts (script)]] — el equivalente para JavaScript.
- [[10 Favicon (link rel icon)]] — el mismo `<link>` con otro `rel`.
