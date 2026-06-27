---
title: Estilos — fillStyle, strokeStyle y propiedades visuales
aliases:
  - fillStyle
  - strokeStyle
  - estilos canvas
  - canvas gradients
tags:
  - javascript
  - api/objeto
  - canvas
objeto: CanvasRenderingContext2D
tipo: concepto
draft: false
order: 3
---

# Estilos (fillStyle, strokeStyle)

> [!definicion]
> Las propiedades de estilo del `CanvasRenderingContext2D` forman parte de su **estado** y afectan a todo lo que se dibuje después de asignarlas. `fillStyle` controla el relleno; `strokeStyle`, el trazo. Ambas aceptan strings de color CSS, objetos `CanvasGradient` y objetos `CanvasPattern`. El resto de propiedades (grosor, extremos, sombras, opacidad, composición) también persisten hasta que se reasignen o se restaure el estado con `restore()`.

```js
ctx.fillStyle   = "#4f46e5";          // color sólido
ctx.strokeStyle = "rgba(0,0,0,0.6)";  // color semitransparente
ctx.lineWidth   = 3;
ctx.fillRect(20, 20, 160, 80);
ctx.strokeRect(20, 20, 160, 80);
```

## Propiedades de color y opacidad

| Propiedad | Tipo de valor | Default | Efecto |
|---|---|---|---|
| `fillStyle` | string CSS / CanvasGradient / CanvasPattern | `"#000000"` | Color/gradiente/patrón de relleno |
| `strokeStyle` | string CSS / CanvasGradient / CanvasPattern | `"#000000"` | Color/gradiente/patrón de trazo |
| `globalAlpha` | número 0–1 | `1` | Opacidad multiplicativa aplicada a todo lo dibujado |

`fillStyle` y `strokeStyle` aceptan cualquier color CSS válido: nombres (`"red"`), hex (`"#ff0000"`, `"#f00"`), `rgb(...)`, `rgba(...)`, `hsl(...)`, `oklch(...)`.

## Propiedades de línea

| Propiedad | Valores posibles | Default | Efecto |
|---|---|---|---|
| `lineWidth` | número > 0 | `1` | Grosor del trazo en píxeles de canvas |
| `lineCap` | `"butt"` / `"round"` / `"square"` | `"butt"` | Extremos de una línea abierta |
| `lineJoin` | `"miter"` / `"round"` / `"bevel"` | `"miter"` | Unión entre dos segmentos |
| `miterLimit` | número | `10` | Longitud máxima del pico de miter antes de cortarlo a bevel |
| `setLineDash(segmentos)` | array de números | `[]` | Patrón de trazos y huecos alternados |
| `lineDashOffset` | número | `0` | Desplazamiento de fase del patrón de trazos |

```js
ctx.setLineDash([10, 5]);   // 10px trazo, 5px hueco
ctx.lineDashOffset = 0;
ctx.strokeRect(20, 20, 200, 100);
```

## Sombras

| Propiedad | Default | Descripción |
|---|---|---|
| `shadowColor` | `"rgba(0,0,0,0)"` (transparente) | Color de la sombra |
| `shadowBlur` | `0` | Radio de desenfoque (sin unidades; no son píxeles exactos) |
| `shadowOffsetX` | `0` | Desplazamiento horizontal de la sombra |
| `shadowOffsetY` | `0` | Desplazamiento vertical de la sombra |

```js
ctx.shadowColor   = "rgba(0,0,0,0.4)";
ctx.shadowBlur    = 12;
ctx.shadowOffsetX = 4;
ctx.shadowOffsetY = 4;
ctx.fillStyle = "white";
ctx.fillRect(40, 40, 180, 90);
// Resetear sombra para que no afecte a lo siguiente:
ctx.shadowColor = "transparent";
```

## Composición global

`globalCompositeOperation` define cómo se mezcla cada píxel nuevo (source) con el que ya está en el bitmap (destination):

| Valor | Efecto visual |
|---|---|
| `"source-over"` | El nuevo píxel se pinta encima (default) |
| `"source-in"` | Solo la intersección; fuera queda transparente |
| `"source-out"` | Lo nuevo donde no hay destination |
| `"destination-over"` | El nuevo píxel queda detrás del existente |
| `"destination-out"` | Borra el área del bitmap donde se dibuja (borrador) |
| `"multiply"` | Multiplica los canales — oscurece |
| `"screen"` | Efecto de exposición doble — aclara |
| `"overlay"` | Combina multiply y screen según la luminosidad |
| `"xor"` | Los píxeles compartidos quedan transparentes |

```js
ctx.fillStyle = "blue";
ctx.fillRect(0, 0, 100, 100);

ctx.globalCompositeOperation = "destination-out";
ctx.fillStyle = "black"; // el color no importa con destination-out
ctx.beginPath();
ctx.arc(80, 80, 50, 0, Math.PI * 2);
ctx.fill(); // borra el área circular del bitmap
ctx.globalCompositeOperation = "source-over"; // restaurar
```

## Gradientes

### Gradiente lineal

```js
const grad = ctx.createLinearGradient(x0, y0, x1, y1);
// (x0,y0): punto donde empieza el gradiente (stop 0)
// (x1,y1): punto donde termina (stop 1)
grad.addColorStop(0,    "#6366f1");
grad.addColorStop(0.5,  "#8b5cf6");
grad.addColorStop(1,    "#ec4899");
ctx.fillStyle = grad;
ctx.fillRect(0, 0, 300, 100);
```

### Gradiente radial

```js
// createRadialGradient(x0, y0, r0, x1, y1, r1)
// círculo interior: centro (x0,y0) radio r0
// círculo exterior: centro (x1,y1) radio r1
const radial = ctx.createRadialGradient(150, 150, 10, 150, 150, 100);
radial.addColorStop(0, "white");
radial.addColorStop(1, "steelblue");
ctx.fillStyle = radial;
ctx.beginPath();
ctx.arc(150, 150, 100, 0, Math.PI * 2);
ctx.fill();
```

## Patrones

```js
const img = new Image();
img.src = "textura.png";
img.onload = () => {
  const patron = ctx.createPattern(img, "repeat"); // "repeat" | "repeat-x" | "repeat-y" | "no-repeat"
  ctx.fillStyle = patron;
  ctx.fillRect(0, 0, canvas.width, canvas.height);
};
```

La fuente de `createPattern` puede ser un `HTMLImageElement`, `HTMLVideoElement`, `HTMLCanvasElement` u `OffscreenCanvas`.

## Receta: botón con gradiente en canvas

```js
function dibujarBoton(ctx, x, y, w, h, texto) {
  const grad = ctx.createLinearGradient(x, y, x, y + h);
  grad.addColorStop(0, "#6366f1");
  grad.addColorStop(1, "#4338ca");

  // Fondo con gradiente
  ctx.fillStyle = grad;
  ctx.beginPath();
  ctx.roundRect(x, y, w, h, 8); // bordes redondeados (Chrome 99+, Firefox 112+)
  ctx.fill();

  // Sombra debajo
  ctx.shadowColor   = "rgba(67,56,202,0.4)";
  ctx.shadowBlur    = 10;
  ctx.shadowOffsetY = 4;

  // Texto centrado
  ctx.shadowColor = "transparent"; // sin sombra en el texto
  ctx.fillStyle    = "white";
  ctx.font         = "bold 16px system-ui";
  ctx.textAlign    = "center";
  ctx.textBaseline = "middle";
  ctx.fillText(texto, x + w / 2, y + h / 2);
}

dibujarBoton(ctx, 50, 50, 200, 50, "Guardar");
```

## Cómo funciona por dentro

Las propiedades de estilo forman parte del **estado del contexto**, que es una estructura plana de valores escalares y referencias a gradientes/patrones. `save()` apila una copia completa del estado (incluyendo todos los estilos); `restore()` la desapila. Las propiedades no se "acumulan": asignar `fillStyle` reemplaza el valor anterior. Los gradientes y patrones son objetos independientes del contexto — se crean con `createLinearGradient` / `createPattern` y se asignan como referencias.

> [!warning]
> Las sombras añaden coste de renderizado significativo y se aplican a **cualquier cosa dibujada**, incluyendo texto e imágenes. Siempre resetear `shadowColor = "transparent"` (o `ctx.save()`/`ctx.restore()`) antes de dibujar elementos que no deben tener sombra.

> [!tip]
> `globalAlpha` multiplica la opacidad de todo lo dibujado, incluyendo los valores alpha ya presentes en `fillStyle`/`strokeStyle`. Para aplicar opacidad solo a un grupo de elementos, usar `ctx.save()`, ajustar `globalAlpha`, dibujar, y luego `ctx.restore()`.

## Notas relacionadas

- [[02 Dibujar Formas y Trazados]] — los paths que reciben el estilo.
- [[05 Transformaciones]] — `save`/`restore` para aislar estilos por grupo de dibujo.
- [[index|Canvas API]] — visión general.
