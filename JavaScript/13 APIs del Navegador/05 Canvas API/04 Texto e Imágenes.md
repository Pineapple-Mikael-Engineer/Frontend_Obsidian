---
title: Texto e Imágenes — Canvas 2D
aliases:
  - fillText canvas
  - drawImage canvas
  - getImageData
  - canvas texto
  - canvas imagen
tags:
  - javascript
  - api/objeto
  - canvas
objeto: CanvasRenderingContext2D
tipo: concepto
draft: false
order: 4
---

# Texto e Imágenes

> [!definicion]
> El contexto 2D puede renderizar texto directamente sobre el bitmap con `fillText`/`strokeText` y dibujar imágenes (o frames de vídeo, otros canvas) con `drawImage`. Para acceso a nivel de píxel, `getImageData` devuelve un array RGBA plano que se puede manipular y reescribir con `putImageData`.

```js
// Texto
ctx.font = "bold 24px system-ui";
ctx.fillStyle = "#1e1b4b";
ctx.fillText("Canvas", 50, 50);

// Imagen
const img = new Image();
img.src = "foto.jpg";
img.onload = () => ctx.drawImage(img, 0, 0);
```

## Texto

### Propiedades de fuente y alineación

| Propiedad | Valores posibles | Default | Efecto |
|---|---|---|---|
| `ctx.font` | string de fuente CSS | `"10px sans-serif"` | Fuente, peso, tamaño y familia |
| `ctx.textAlign` | `"left"` / `"center"` / `"right"` / `"start"` / `"end"` | `"start"` | Alineación horizontal relativa al punto x de `fillText` |
| `ctx.textBaseline` | `"top"` / `"hanging"` / `"middle"` / `"alphabetic"` / `"ideographic"` / `"bottom"` | `"alphabetic"` | Línea base vertical relativa al punto y de `fillText` |
| `ctx.direction` | `"ltr"` / `"rtl"` / `"inherit"` | `"inherit"` | Dirección del texto |

### Métodos de renderizado de texto

| Método | Retorna | Descripción |
|---|---|---|
| `ctx.fillText(texto, x, y, maxAncho?)` | `undefined` | Texto relleno con `fillStyle` |
| `ctx.strokeText(texto, x, y, maxAncho?)` | `undefined` | Contorno del texto con `strokeStyle` |
| `ctx.measureText(texto)` | `TextMetrics` | Medición tipográfica del texto con la fuente actual |

`maxAncho` es opcional: si se especifica, el navegador comprime el texto horizontalmente para que quepa.

### TextMetrics

`ctx.measureText(texto)` retorna un objeto `TextMetrics`. La propiedad más usada es `width`; el resto requiere soporte del navegador:

```js
const metrics = ctx.measureText("Hola mundo");
console.log(metrics.width);                // ancho en píxeles
console.log(metrics.actualBoundingBoxAscent);  // por encima de la baseline
console.log(metrics.actualBoundingBoxDescent); // por debajo de la baseline
```

### Texto centrado en un rectángulo

```js
function textoEnRecuadro(ctx, texto, x, y, w, h) {
  ctx.fillStyle = "#f1f5f9";
  ctx.fillRect(x, y, w, h);

  ctx.fillStyle = "#0f172a";
  ctx.font = "16px system-ui";
  ctx.textAlign = "center";
  ctx.textBaseline = "middle";
  ctx.fillText(texto, x + w / 2, y + h / 2, w - 16);
}
```

## Imágenes

### Formas de drawImage

`drawImage` acepta tres signaturas:

| Parámetros | Descripción |
|---|---|
| `(fuente, dx, dy)` | Dibuja la imagen en (dx, dy) con su tamaño original |
| `(fuente, dx, dy, dw, dh)` | Dibuja la imagen redimensionada a dw×dh en (dx, dy) |
| `(fuente, sx, sy, sw, sh, dx, dy, dw, dh)` | Recorta el área (sx,sy,sw,sh) del source y la dibuja en el destino (dx,dy,dw,dh) |

La fuente puede ser: `HTMLImageElement`, `HTMLVideoElement`, `HTMLCanvasElement`, `ImageBitmap`, `OffscreenCanvas`, `SVGImageElement`.

```js
// Con recorte: extraer un sprite de 64×64 de una spritesheet
ctx.drawImage(
  spritesheet,
  64, 0,   // sx, sy: columna 2, fila 1 del sprite
  64, 64,  // sw, sh: tamaño del sprite
  100, 100, // dx, dy: posición en el canvas
  64, 64   // dw, dh: tamaño en el canvas
);
```

### Cargar imágenes antes de dibujar

```js
function cargarImagen(src) {
  return new Promise((resolve, reject) => {
    const img = new Image();
    img.onload  = () => resolve(img);
    img.onerror = reject;
    img.src = src;
  });
}

const img = await cargarImagen("foto.jpg");
ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
```

## Acceso a píxeles

### getImageData

```js
// Retorna un ImageData con los píxeles del área rectangular
const imageData = ctx.getImageData(x, y, width, height);
// imageData.data: Uint8ClampedArray de longitud width * height * 4
// Cada grupo de 4 bytes es [R, G, B, A] de un píxel, en orden de raster (izquierda→derecha, arriba→abajo)
```

### putImageData

```js
ctx.putImageData(imageData, dx, dy);
// Escribe el ImageData en el canvas en (dx, dy)
// También acepta parámetros de recorte: putImageData(data, dx, dy, dirtyX, dirtyY, dw, dh)
```

### Receta: filtro de escala de grises

```js
function escalaDeGrises(ctx, x, y, w, h) {
  const data = ctx.getImageData(x, y, w, h);
  const px   = data.data; // Uint8ClampedArray

  for (let i = 0; i < px.length; i += 4) {
    // Luminancia perceptual (pesos ITU-R BT.601)
    const gris = px[i] * 0.299 + px[i + 1] * 0.587 + px[i + 2] * 0.114;
    px[i]     = gris; // R
    px[i + 1] = gris; // G
    px[i + 2] = gris; // B
    // px[i + 3] = canal alpha, sin cambios
  }

  ctx.putImageData(data, x, y);
}

// Uso: después de dibujar la imagen
ctx.drawImage(img, 0, 0);
escalaDeGrises(ctx, 0, 0, canvas.width, canvas.height);
```

## Cómo funciona por dentro

`fillText` y `strokeText` renderizan el texto usando el motor de fuentes del navegador sobre el bitmap del canvas. El texto rasterizado es píxeles — no hay nodos de texto ni accesibilidad automática. `drawImage` copia el bitmap de la fuente al canvas aplicando la transformación actual; si la fuente es un `HTMLVideoElement`, captura el frame visible en ese momento.

`getImageData` accede al bitmap raw: retorna un `Uint8ClampedArray` (valores enteros 0–255, sin desbordamiento). Las operaciones sobre este array son síncronas y pueden ser lentas para canvas grandes; para procesado de imagen de alto rendimiento, `OffscreenCanvas` con un Worker es la alternativa.

> [!warning]
> `getImageData` y `drawImage` están sujetos a la política de **mismo origen** (CORS). Intentar llamar a `getImageData` después de dibujar una imagen cross-origin sin los headers CORS adecuados lanza un `SecurityError` y convierte el canvas en "tainted" (contaminado) — hasta que se recrea el canvas, todas las llamadas a `getImageData` y `toDataURL` fallarán.

> [!tip]
> `createImageBitmap(fuente)` crea un `ImageBitmap` fuera del hilo principal (acepta Blobs, respuestas fetch, canvas, vídeo) y lo transfiere a un Worker para procesar. Es más eficiente que `getImageData` para pipelines de procesado de imagen asíncrono.

## Notas relacionadas

- [[03 Estilos (fillStyle, strokeStyle)]] — `fillStyle` y `font` afectan al texto; `globalCompositeOperation` afecta a `drawImage`.
- [[05 Transformaciones]] — las transformaciones activas en el contexto se aplican tanto a `fillText` como a `drawImage`.
- [[index|Canvas API]] — visión general.
