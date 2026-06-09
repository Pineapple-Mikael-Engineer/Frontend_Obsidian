---
title: Contexto 2D — CanvasRenderingContext2D
aliases:
  - getContext 2d
  - CanvasRenderingContext2D
  - contexto de canvas
tags:
  - javascript
  - api/objeto
  - canvas
objeto: HTMLCanvasElement
tipo: concepto
draft: false
---

# Contexto 2D

> [!definicion]
> `canvas.getContext("2d")` retorna un objeto `CanvasRenderingContext2D` que es la interfaz de dibujo 2D sobre el bitmap del canvas. Todos los métodos de dibujo, estilos y transformaciones se invocan sobre este contexto. El elemento `<canvas>` en sí solo expone la superficie; el contexto es quien provee la API. Si el contexto ya fue creado, `getContext` retorna la misma instancia.

```js
const canvas = document.querySelector("canvas"); // o document.createElement("canvas")
const ctx = canvas.getContext("2d");

// Limpiar todo el canvas antes de re-dibujar
ctx.clearRect(0, 0, canvas.width, canvas.height);
```

## Atributos del elemento `<canvas>`

`<canvas>` acepta dos atributos que definen los **píxeles lógicos** del bitmap:

| Atributo | Tipo | Default | Descripción |
|---|---|---|---|
| `width` | número (px) | 300 | Ancho del bitmap en píxeles de canvas |
| `height` | número (px) | 150 | Alto del bitmap en píxeles de canvas |

Estos atributos son distintos al tamaño CSS. Sin ellos el canvas mide 300×150px por defecto.

```html
<canvas width="800" height="600"></canvas>
```

## Tamaño del canvas vs tamaño CSS

`canvas.width` / `canvas.height` definen los **píxeles lógicos** del bitmap. `canvas.style.width` / `canvas.style.height` (o reglas CSS) definen cómo se escala visualmente en la página. Son independientes.

```js
canvas.width  = 800; // el bitmap tiene 800 columnas de píxeles
canvas.style.width = "400px"; // el canvas se muestra en 400px CSS → factor 2x
```

Al escalar el bitmap a un tamaño CSS menor, el canvas aparece más nítido porque hay más píxeles por punto CSS. Al escalarlo a uno mayor, se verá pixelado.

### Pantallas Retina / HiDPI

En pantallas con `devicePixelRatio > 1`, hacer el canvas el doble de grande en píxeles lógicos y reducirlo a la mitad con CSS produce gráficos nítidos:

```js
const dpr = window.devicePixelRatio || 1;
const rect = canvas.getBoundingClientRect();

canvas.width  = rect.width  * dpr;
canvas.height = rect.height * dpr;

canvas.style.width  = rect.width  + "px";
canvas.style.height = rect.height + "px";

const ctx = canvas.getContext("2d");
ctx.scale(dpr, dpr); // todos los dibujos usan coordenadas CSS normales
```

## Sistema de coordenadas

El origen (0, 0) está en la **esquina superior izquierda**. `x` crece hacia la derecha; `y` crece hacia abajo. Las unidades son píxeles del canvas (no píxeles CSS si hay escala DPR activa).

```
(0,0) ──────────→ x
  │
  │
  ↓
  y
```

## Métodos de limpieza y exportación

| Método | Retorna | Descripción |
|---|---|---|
| `ctx.clearRect(x, y, w, h)` | `undefined` | Borra el área rectangular (píxeles a transparente) |
| `canvas.toDataURL(type?, calidad?)` | `string` (base64) | Exporta el bitmap como data URL |
| `canvas.toBlob(callback, type?, calidad?)` | `undefined` | Exporta como `Blob`; el callback recibe el Blob |

```js
// Limpiar todo el canvas
ctx.clearRect(0, 0, canvas.width, canvas.height);

// Exportar como PNG (data URL para mostrar en <img> o descargar)
const dataURL = canvas.toDataURL("image/png");
const img = new Image();
img.src = dataURL;

// Exportar como Blob (más eficiente para descarga)
canvas.toBlob(blob => {
  const url = URL.createObjectURL(blob);
  const a = document.createElement("a");
  a.href = url;
  a.download = "figura.png";
  a.click();
  URL.revokeObjectURL(url);
}, "image/png");
```

## Cómo funciona por dentro

El `CanvasRenderingContext2D` mantiene un **estado** interno que incluye: transformación actual (matriz 2D), estilos de relleno y trazo, fuente, opacidad global, clipping path, y la pila de estados guardados con `save`/`restore`. Cada operación de dibujo se compone sobre el bitmap: a diferencia del DOM, no hay escena que se pueda re-consultar ni objetos que persistan. El contexto escribe píxeles y olvida.

Cuando `canvas.width` o `canvas.height` se reasignan (incluso al mismo valor), el bitmap se borra completamente y el estado del contexto se resetea a sus valores por defecto.

> [!warning]
> Reasignar `canvas.width = canvas.width` borra el canvas y resetea el contexto: se pierden todos los estilos, transformaciones y la escala DPR aplicada. Si el canvas necesita redimensionarse dinámicamente, hay que volver a aplicar `ctx.scale(dpr, dpr)` y restaurar los estilos.

> [!tip]
> Para crear un canvas fuera del DOM (buffer de renderizado), `document.createElement("canvas")` produce un canvas válido al que se puede llamar `getContext("2d")` sin necesidad de insertarlo en el documento.

## Notas relacionadas

- [[05 Transformaciones]] — `ctx.save()` / `ctx.restore()` y la pila de estados.
- [[04 Texto e Imágenes]] — `getImageData` / `putImageData` para acceso directo a píxeles.
- [[index|Canvas API]] — visión general de todos los submódulos.
