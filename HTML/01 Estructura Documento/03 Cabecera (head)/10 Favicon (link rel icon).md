---
title: <link rel="icon"> — Favicon de la pestaña
aliases:
  - favicon
  - icon link
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

# Favicon (link rel icon)

> [!definicion]
> `<link rel="icon" href="…">` define el **icono** que el navegador muestra en la pestaña, los
> marcadores y el historial. Es el mismo elemento [[07 Enlace a CSS (link) | `<link>`]] con
> `rel="icon"`.

```html
<head>
  <link rel="icon" href="/favicon.ico" sizes="any" />
  <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
  <link rel="apple-touch-icon" href="/apple-touch-icon.png" />
</head>
```

## Variantes habituales

| `rel` | Uso |
|-------|-----|
| `icon` | Favicon estándar de la pestaña |
| `apple-touch-icon` | Icono al añadir a pantalla de inicio en iOS |
| `mask-icon` | Icono monocromo para pestañas fijadas en Safari |

| `type` | Formato |
|--------|---------|
| `image/x-icon` | `.ico` clásico (multi-resolución) |
| `image/svg+xml` | SVG escalable (recomendado hoy) |
| `image/png` | PNG a tamaños concretos (`sizes="32x32"`) |

> [!info] El navegador busca /favicon.ico por defecto
> Aunque no se declare el `<link>`, el navegador pide automáticamente `/favicon.ico` en la raíz. Por
> eso muchos sitios "tienen favicon" sin etiqueta. Declararlo explícitamente da control sobre formato
> y resolución.

> [!tip] SVG + ICO de respaldo
> Un favicon SVG escala perfecto y soporta modo oscuro con `prefers-color-scheme` dentro del propio
> SVG; se acompaña de un `.ico` para navegadores que no admiten SVG como icono.

## Notas relacionadas

- [[07 Enlace a CSS (link)]] — el mismo elemento con otro `rel`.
- [[11 Enlace Canónico (link rel canonical)]] — otra variante de `<link>`.
