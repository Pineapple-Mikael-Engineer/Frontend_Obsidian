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
> marcadores, el historial y la pantalla de inicio al instalar la web. Es el mismo elemento
> [[07 Enlace a CSS (link) | `<link>`]] con `rel="icon"`, y en la práctica se declara en varias
> variantes para cubrir todos los contextos.

```html
<head>
  <link rel="icon" href="/favicon.ico" sizes="any" />
  <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
  <link rel="apple-touch-icon" href="/apple-touch-icon.png" />
</head>
```

## Variantes por contexto

Cada plataforma busca un `rel`/tamaño distinto. Un set moderno y razonable cubre lo esencial sin
generar las decenas de archivos de antaño:

| `rel` | Uso | Formato recomendado |
|-------|-----|---------------------|
| `icon` | Favicon estándar de la pestaña | SVG + `.ico` de respaldo |
| `apple-touch-icon` | Icono al "Añadir a pantalla de inicio" en iOS | PNG 180×180 |
| `mask-icon` | Pestaña fijada en Safari (monocromo) | SVG |
| (manifest) | Iconos de PWA instalada | PNG vía `manifest.json` |

### Formatos

| `type` | Formato | Nota |
|--------|---------|------|
| `image/svg+xml` | SVG escalable | **Recomendado hoy**: nítido a cualquier tamaño |
| `image/x-icon` | `.ico` clásico | Contenedor multi-resolución, máxima compatibilidad |
| `image/png` | PNG a tamaño concreto | `sizes="32x32"`, `sizes="16x16"` |

## El comportamiento por defecto

> [!info] El navegador busca /favicon.ico solo
> Aunque no declares ningún `<link rel="icon">`, el navegador pide automáticamente `/favicon.ico` en
> la raíz del dominio. Por eso muchos sitios "tienen favicon" sin etiqueta. Declararlo explícitamente
> da control sobre **formato, resolución y modo oscuro**, y evita un 404 silencioso en los logs.

## SVG con modo oscuro

Un favicon SVG puede adaptarse al tema del sistema incluyendo una media query **dentro del propio
SVG**:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
  <style>
    path { fill: #1e1e2e; }
    @media (prefers-color-scheme: dark) { path { fill: #cba6f7; } }
  </style>
  <path d="…" />
</svg>
```

## Buenas prácticas

> [!tip] Set mínimo recomendado
> - Un **SVG** para la pestaña (escala perfecto, soporta modo oscuro).
> - Un **`.ico`** de respaldo para navegadores que no aceptan SVG como icono.
> - Un **PNG 180×180** como `apple-touch-icon` para iOS.
> - Para PWA, declara los iconos en `manifest.json`, no solo aquí.

## Errores comunes

> [!warning] Trampas
> - **Confiar solo en `/favicon.ico`** sin `apple-touch-icon`: iOS muestra una miniatura genérica al
>   guardar en pantalla de inicio.
> - **Caché agresiva**: los favicons se cachean con fuerza; al cambiarlo conviene versionar el nombre
>   (`favicon.svg?v=2`) o purgar la caché.
> - **Olvidar `sizes`/`type`**: ayuda al navegador a elegir el icono correcto sin descargar todos.

## Notas relacionadas

- [[07 Enlace a CSS (link)]] — el mismo elemento `<link>` con otro `rel`.
- [[11 Enlace Canónico (link rel canonical)]] — otra variante de `<link>` en el head.
