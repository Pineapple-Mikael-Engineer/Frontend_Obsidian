---
title: Canvas API — dibujo 2D (y 3D) en el navegador
aliases:
  - Canvas API
  - HTMLCanvasElement
  - canvas
tags:
  - javascript
  - api/web
  - canvas
draft: false
---

# Canvas API

> [!definicion]
> La **Canvas API** expone un elemento `<canvas>` que actúa como un **bitmap mutable** sobre el que se puede dibujar mediante scripts. A diferencia de SVG, el canvas no tiene árbol de elementos — cada píxel se escribe directamente sin crear nodos en el DOM, lo que lo hace adecuado para gráficos densos, animaciones y procesado de imagen. El contexto de dibujo se obtiene con `canvas.getContext("2d")` (o `"webgl"` / `"webgl2"` para 3D).

```js
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");

ctx.fillStyle = "#4f46e5";
ctx.fillRect(20, 20, 200, 100);

ctx.fillStyle = "white";
ctx.font = "bold 18px sans-serif";
ctx.textAlign = "center";
ctx.textBaseline = "middle";
ctx.fillText("Canvas API", 120, 70);
```

## Tipos de contexto

| Tipo | Uso | API principal |
|---|---|---|
| `"2d"` | Gráficos 2D, animaciones, procesado de imagen | `CanvasRenderingContext2D` |
| `"webgl"` | 3D acelerado por GPU (WebGL 1) | `WebGLRenderingContext` |
| `"webgl2"` | 3D acelerado por GPU (WebGL 2) | `WebGL2RenderingContext` |
| `"bitmaprenderer"` | Transferir un `ImageBitmap` directamente al canvas | `ImageBitmapRenderingContext` |

## Contenido de esta sección

- [[01 Contexto 2D]] — obtener el contexto, coordenadas, tamaño vs CSS, exportar.
- [[02 Dibujar Formas y Trazados]] — rectángulos directos, path API (`beginPath`, `arc`, `bezierCurveTo`…), `fill`/`stroke`.
- [[03 Estilos (fillStyle, strokeStyle)]] — colores, grosores, gradientes, patrones, sombras, `globalCompositeOperation`.
- [[04 Texto e Imágenes]] — `fillText`, `measureText`, `drawImage` con recorte, `getImageData`/`putImageData`.
- [[05 Transformaciones]] — `translate`, `rotate`, `scale`, matrices, `save`/`restore`, animación con `requestAnimationFrame`.

## Canvas vs SVG

| Criterio | Canvas | SVG |
|---|---|---|
| Modelo | Bitmap (píxeles) | Árbol de elementos vectoriales |
| Escalado | Pierde calidad al aumentar | Escala sin pérdida |
| Interactividad por elemento | Requiere cálculo manual (hit-test) | Nativa (event listeners en nodos) |
| Rendimiento con muchos objetos | Mejor (no hay DOM overhead) | Peor (cada objeto es un nodo) |
| Animación de píxeles | Nativa | Costosa |
| Accesibilidad | Limitada (requiere ARIA manual) | Buena (nodos son semánticos) |

Canvas es la elección natural para juegos, visualizaciones de datos con miles de elementos, editores de imagen y procesado píxel a píxel. SVG es preferible para iconos, ilustraciones escalables y gráficos con pocos objetos interactivos.

> [!tip]
> `OffscreenCanvas` permite delegar el renderizado a un Web Worker, liberando el hilo principal. Se crea con `new OffscreenCanvas(width, height)` o transfiriendo un canvas con `canvas.transferControlToOffscreen()`.

> [!warning]
> El canvas es un bitmap: no hay forma de recuperar un "objeto" ya dibujado. Para escenas con elementos que necesitan moverse o responder a eventos de forma independiente, es necesario mantener un modelo propio de los objetos en JavaScript y re-renderizar el canvas completo en cada frame.

## Notas relacionadas

- [[../01 Window/index|Window]] — `requestAnimationFrame` vive en `window` y es el mecanismo correcto para animaciones en canvas.
- [[../06 Observers/index|Observers]] — `ResizeObserver` para redimensionar el canvas cuando cambia el contenedor.
