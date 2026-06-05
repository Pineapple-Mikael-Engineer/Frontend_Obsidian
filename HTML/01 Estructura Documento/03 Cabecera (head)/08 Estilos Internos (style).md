---
title: <style> — Estilos CSS embebidos en el documento
aliases:
  - style element
  - internal css
tags:
  - html
  - api/elemento
  - metadatos
elemento: style
categoria: metadatos
rol_implicito: ninguno
vacio: false
draft: false
---

# Estilos Internos (style)

> [!definicion]
> `<style>` incrusta reglas CSS **dentro del propio HTML**, normalmente en el [[index | `<head>`]].
> A diferencia de [[07 Enlace a CSS (link) | `<link>`]], el CSS viaja con el documento: no hay
> petición extra, pero tampoco se cachea ni se comparte entre páginas.

```html
<head>
  <style>
    body { margin: 0; font-family: system-ui; }
    .destacado { color: crimson; }
  </style>
</head>
```

## Cuándo usarlo

| Caso | Preferir |
|------|----------|
| Estilos de un sitio completo | `<link>` externo (cacheable, reutilizable) |
| CSS crítico inline para el primer pintado | `<style>` en el `<head>` |
| Email HTML | `<style>` o estilos inline (no hay archivos externos) |
| Prototipo rápido / demo de una página | `<style>` |

> [!tip] CSS crítico above-the-fold
> Una técnica de rendimiento: incrustar con `<style>` el CSS mínimo de lo visible al cargar, y
> diferir el resto con `<link>`. Elimina el render-blocking de la hoja completa sin sacrificar el
> primer pintado.

> [!warning] No es lo mismo que el style inline
> `<style>` (un bloque de reglas en el `<head>`) ≠ el atributo `style="…"` en un elemento. Este
> último, [[02 Estilo en Línea (style) | estilo en línea]], aplica a un solo elemento, tiene
> altísima especificidad y se considera mala práctica salvo casos muy puntuales.

## Notas relacionadas

- [[07 Enlace a CSS (link)]] — la alternativa externa, preferida en producción.
- [[02 Estilo en Línea (style)]] — el atributo `style` por elemento (atributo global).
