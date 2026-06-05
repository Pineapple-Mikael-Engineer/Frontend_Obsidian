---
title: <meta viewport> — Control del viewport en móviles
aliases:
  - viewport
  - responsive meta tag
tags:
  - html
  - api/elemento
  - responsive
elemento: meta
categoria: metadatos
rol_implicito: ninguno
vacio: true
draft: false
---

# Viewport (meta viewport)

> [!definicion]
> `<meta name="viewport">` indica al navegador móvil cómo ajustar el **área visible** (viewport) al
> ancho real del dispositivo. Sin ella, el móvil asume un viewport de ~980px y **escala la página**
> hacia abajo, haciéndola diminuta y obligando a hacer zoom.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

Esta es la línea estándar y suficiente para diseño responsivo: el ancho del viewport iguala al
ancho del dispositivo, sin zoom inicial.

## Directivas de `content`

| Directiva | Valor típico | Efecto |
|-----------|--------------|--------|
| `width` | `device-width` | Iguala el viewport al ancho físico del dispositivo |
| `initial-scale` | `1.0` | Nivel de zoom al cargar (1 = sin zoom) |
| `minimum-scale` | `1.0` | Zoom mínimo permitido |
| `maximum-scale` | `5.0` | Zoom máximo permitido |
| `user-scalable` | `yes` / `no` | Si el usuario puede hacer pinch-zoom |

> [!warning] No deshabilitar el zoom
> `user-scalable=no` o `maximum-scale=1.0` impiden que el usuario amplíe la página. Es una
> **barrera de accesibilidad** grave para personas con baja visión y viola WCAG 1.4.4. Salvo casos
> muy concretos (un mapa interactivo), dejar el zoom libre.

> [!info] Es el cimiento del responsive
> Sin esta etiqueta, las media queries de CSS no se comportan como se espera en móvil, porque el
> viewport reportado no coincide con el dispositivo. Es el prerrequisito de cualquier
> [[03 CSS Grid/index | layout]] adaptable (delegado a CSS).

## Notas relacionadas

- [[01 Codificación de Caracteres (meta charset)]] — el otro `<meta>` que va arriba.
- [[index]] — el `<head>` completo.
