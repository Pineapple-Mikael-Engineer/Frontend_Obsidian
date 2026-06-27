---
title: CSS Interno (style) — Estilos embebidos en el documento
aliases:
  - internal css
  - style tag
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 2
---

# CSS Interno (style)

> [!definicion]
> El CSS **interno** se escribe dentro de un elemento `<style>` en el `<head>` del HTML. Las reglas viajan con el documento: no hay petición de red extra, pero tampoco se cachean ni se comparten entre páginas.

```html
<head>
  <style>
    body { margin: 0; font-family: system-ui; }
    .destacado { color: crimson; }
  </style>
</head>
```

## Cuándo usarlo

| Caso | Por qué |
|------|---------|
| CSS crítico inicial | Pintar lo visible sin esperar a la hoja externa |
| Email HTML | Los clientes de correo no cargan archivos externos |
| Página única / demo | Todo en un archivo, por comodidad |
| Prototipo rápido | Sin montar la estructura de archivos |

## La técnica del CSS crítico

> [!tip] Inline lo crítico, difiere el resto
> Una optimización real de rendimiento: incrustar con `<style>` el CSS mínimo de lo visible al cargar (*above the fold*) y cargar el resto de la hoja de forma diferida. Así el primer pintado no espera la hoja completa:
> ```html
> <style>/* CSS crítico: layout y tipografía base */</style>
> <link rel="preload" href="resto.css" as="style" onload="this.rel='stylesheet'" />
> ```

## No es lo mismo que el style en línea

> [!warning] <style> ≠ atributo style
> Distingue:
> - El **elemento** `<style>` (esta nota): un bloque de **reglas** reutilizables con selectores.
> - El **atributo** [[03 CSS en Línea (atributo style) | `style="…"`]]: estilos aplicados a **un** elemento concreto, sin selector, con especificidad altísima y mala práctica.
>
> Tienen el mismo nombre pero son cosas distintas.

## Buenas prácticas

> [!tip] Recomendaciones
> - En producción, externo por defecto; `<style>` solo para CSS crítico o email.
> - Mantén el `<style>` en el `<head>`, no esparcido por el `<body>`.
> - El atributo `media` también funciona: `<style media="print">`.

## Errores comunes

> [!warning] Trampas
> - **CSS interno duplicado** en cada página: se pierde la ventaja de la caché.
> - **Confundirlo con el atributo `style`**.

## Notas relacionadas

- [[01 CSS Externo (link)]] — la alternativa preferida en producción.
- [[03 CSS en Línea (atributo style)]] — el atributo `style`, no confundir.
- [[08 Estilos Internos (style)]] — el elemento `<style>` desde HTML.
