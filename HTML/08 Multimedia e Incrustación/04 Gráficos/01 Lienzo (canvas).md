---
title: <canvas> — Lienzo de dibujo por píxeles
aliases:
  - canvas element
tags:
  - html
  - api/elemento
  - multimedia
elemento: canvas
categoria: incrustado
rol_implicito: ninguno
vacio: false
draft: false
---

# Lienzo (canvas)

> [!definicion]
> `<canvas>` es un **lienzo de píxeles** sobre el que se dibuja con JavaScript: gráficos, animaciones, juegos, visualizaciones, manipulación de imágenes. El elemento HTML solo crea el área en blanco; todo el dibujo ocurre mediante la **Canvas API** desde código.

```html
<canvas id="lienzo" width="400" height="300">
  Tu navegador no soporta canvas.
</canvas>
```

```js
const ctx = document.getElementById("lienzo").getContext("2d");
ctx.fillStyle = "#cba6f7";
ctx.fillRect(50, 50, 100, 80);   // un rectángulo
```

## width/height del atributo vs. CSS

> [!warning] No fijes el tamaño del canvas solo con CSS
> El `<canvas>` tiene dos tamaños distintos:
> - Los atributos `width`/`height` definen la **resolución del lienzo** (cuántos píxeles de dibujo).
> - El `width`/`height` de **CSS** define el tamaño **visual** en pantalla.
>
> Si solo pones el tamaño con CSS, el lienzo mantiene su resolución por defecto (300×150) y se **estira**, deformando el dibujo. Define la resolución con los atributos y, si acaso, el tamaño visual con CSS de forma coherente:
> ```html
> <canvas width="800" height="600" style="width:400px;height:300px"></canvas>
> ```

## getContext: el punto de entrada

Todo el dibujo pasa por un **contexto** obtenido con `getContext()`:

| Contexto | Para qué |
|----------|----------|
| `"2d"` | Dibujo 2D (formas, texto, imágenes) |
| `"webgl"` / `"webgl2"` | Gráficos 3D acelerados por hardware |
| `"webgpu"` | API moderna de GPU (más reciente) |

## El dibujo vive en JavaScript

> [!info] canvas es el lienzo, no el pincel
> El elemento `<canvas>` por sí solo es un rectángulo vacío. Dibujar líneas, formas, texto, imágenes y animar es trabajo de la **Canvas API** en JavaScript (`fillRect`, `arc`, `drawImage`, `requestAnimationFrame`…). Ese desarrollo pertenece al curso de [[05 Canvas API/index | JavaScript]]; esta nota cubre el elemento HTML.

## Accesibilidad: el gran punto débil

> [!warning] canvas es un agujero de accesibilidad
> Como el canvas es solo píxeles, su contenido es **invisible** para los lectores de pantalla: no hay DOM que describa lo dibujado. Mitigaciones:
> - Poner contenido de respaldo (HTML accesible) **dentro** del `<canvas>`, que describa lo que muestra.
> - Para gráficos importantes, ofrecer una alternativa (una tabla de datos, texto).
> - Si la accesibilidad es prioritaria y las formas son pocas, **SVG es mejor** porque cada forma está en el DOM.

## Buenas prácticas

> [!tip] Recomendaciones
> - Define la resolución con los atributos `width`/`height`, no solo con CSS.
> - Pon contenido de respaldo accesible dentro del `<canvas>`.
> - Usa canvas para muchos objetos o manipulación de píxeles; para pocas formas escalables, SVG.
> - Para pantallas retina, escala la resolución por el `devicePixelRatio`.

## Errores comunes

> [!warning] Trampas
> - **Tamaño solo por CSS**: el dibujo se estira/deforma.
> - **Sin alternativa accesible**: el contenido es invisible para la asistencia.
> - **Usar canvas para un logo o icono** estático: SVG sería más nítido y ligero.

## Notas relacionadas

- [[02 Gráficos Vectoriales (svg)]] — la alternativa vectorial y accesible.
- [[05 Canvas API/index]] — el dibujo con JavaScript (delegado a JS).
- [[01 Lienzo (canvas)]] — referencia HTML del elemento (esta nota).
