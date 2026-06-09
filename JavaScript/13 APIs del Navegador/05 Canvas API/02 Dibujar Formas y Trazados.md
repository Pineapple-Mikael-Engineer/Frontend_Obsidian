---
title: Dibujar Formas y Trazados — Canvas 2D
aliases:
  - Canvas paths
  - beginPath
  - trazados canvas
tags:
  - javascript
  - api/objeto
  - canvas
objeto: CanvasRenderingContext2D
tipo: concepto
draft: false
---

# Dibujar Formas y Trazados

> [!definicion]
> El contexto 2D distingue dos categorías de formas: los **rectángulos** (los únicos que se dibujan directamente en el bitmap sin crear un trazado) y los **trazados** (paths), que acumulan una secuencia de segmentos en un buffer interno y solo se vuelcan al bitmap con `fill()` o `stroke()`. Toda forma no rectangular — círculos, líneas, curvas, polígonos — requiere la API de path.

```js
// Rectángulo relleno directo (sin path)
ctx.fillStyle = "#4f46e5";
ctx.fillRect(10, 10, 150, 80);

// Círculo con path
ctx.beginPath();
ctx.arc(250, 50, 40, 0, Math.PI * 2);
ctx.fillStyle = "#e11d48";
ctx.fill();
```

## Rectángulos (sin path)

| Método | Descripción |
|---|---|
| `ctx.fillRect(x, y, w, h)` | Dibuja un rectángulo relleno con `fillStyle` actual |
| `ctx.strokeRect(x, y, w, h)` | Dibuja el contorno de un rectángulo con `strokeStyle` actual |
| `ctx.clearRect(x, y, w, h)` | Borra el área rectangular (píxeles a transparente) |

Estos métodos actúan inmediatamente sobre el bitmap; no afectan ni son afectados por el path actual.

## API de trazados (paths)

Un path es una lista de segmentos que el contexto mantiene en memoria. El ciclo siempre es: **abrir → construir → cerrar (opcional) → renderizar**.

| Método | Descripción |
|---|---|
| `ctx.beginPath()` | Descarta el path anterior e inicia uno nuevo |
| `ctx.moveTo(x, y)` | Mueve el "cursor" a (x, y) sin trazar ninguna línea |
| `ctx.lineTo(x, y)` | Añade un segmento recto desde la posición actual hasta (x, y) |
| `ctx.arc(x, y, r, inicio, fin, antihor?)` | Arco centrado en (x,y) con radio r, de `inicio` a `fin` en radianes |
| `ctx.arcTo(x1, y1, x2, y2, r)` | Arco tangente a los dos segmentos definidos por el cursor y los dos puntos |
| `ctx.quadraticCurveTo(cpx, cpy, x, y)` | Curva de Bézier cuadrática (un punto de control) |
| `ctx.bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)` | Curva de Bézier cúbica (dos puntos de control) |
| `ctx.rect(x, y, w, h)` | Añade un rectángulo cerrado al path actual |
| `ctx.ellipse(x, y, rx, ry, rot, inicio, fin, antihor?)` | Elipse con semiejes rx/ry rotada `rot` radianes |
| `ctx.closePath()` | Añade un segmento desde la posición actual al punto inicial del subpath |
| `ctx.fill(regla?)` | Rellena el path con `fillStyle` (`"nonzero"` o `"evenodd"`) |
| `ctx.stroke()` | Traza el contorno del path con `strokeStyle` y `lineWidth` |

### Ángulos en `arc`

Los ángulos siguen la convención de trigonometría pero el eje Y está invertido (crece hacia abajo): 0 radianes es la dirección derecha (3 en el reloj), `Math.PI` es la izquierda, `Math.PI / 2` es abajo.

```
         0 (→)
   3π/2  ·  π/2
         π (←)
```

## Receta: triángulo

```js
ctx.beginPath();
ctx.moveTo(100, 10);   // vértice superior
ctx.lineTo(190, 160);  // vértice inferior derecho
ctx.lineTo(10, 160);   // vértice inferior izquierdo
ctx.closePath();       // cierra de vuelta al primer punto

ctx.fillStyle = "#f59e0b";
ctx.fill();
ctx.strokeStyle = "#92400e";
ctx.lineWidth = 3;
ctx.stroke();
```

## Receta: círculo relleno

```js
ctx.beginPath();
ctx.arc(150, 150, 80, 0, Math.PI * 2); // círculo completo
ctx.fillStyle = "steelblue";
ctx.fill();
```

## Receta: polígono genérico

```js
function poligono(ctx, cx, cy, radio, lados) {
  ctx.beginPath();
  for (let i = 0; i < lados; i++) {
    const angulo = (i / lados) * Math.PI * 2 - Math.PI / 2;
    const x = cx + radio * Math.cos(angulo);
    const y = cy + radio * Math.sin(angulo);
    i === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
  }
  ctx.closePath();
}

poligono(ctx, 200, 200, 80, 6); // hexágono
ctx.fillStyle = "#10b981";
ctx.fill();
```

## Cómo funciona por dentro

El path es una estructura interna del contexto (no forma parte del DOM ni del bitmap hasta que se llama a `fill` o `stroke`). Cada llamada a `beginPath()` vacía este buffer; si se omite, los subpaths se acumulan y `fill`/`stroke` operan sobre todos ellos a la vez, produciendo artefactos visuales difíciles de depurar.

`fill()` y `stroke()` pueden llamarse ambos sobre el mismo path: primero se llama a `fill()`, luego a `stroke()` (el trazo se dibuja encima del relleno, centrado sobre el borde matemático de la forma).

> [!warning]
> Olvidar `ctx.beginPath()` antes de empezar una nueva forma es el error más común: el path del frame anterior se acumula y la siguiente `fill()` o `stroke()` pinta todo junto. Siempre abrir un path nuevo antes de cada forma independiente.

> [!tip]
> Para formas complejas reutilizables, `Path2D` permite construir un path una vez y reutilizarlo: `const p = new Path2D(); p.arc(0, 0, 50, 0, Math.PI * 2); ctx.fill(p);`. También acepta strings de path SVG (`new Path2D("M 10 10 L 100 100")`).

## Notas relacionadas

- [[03 Estilos (fillStyle, strokeStyle)]] — `fillStyle`, `strokeStyle`, `lineWidth` y demás propiedades que determinan el aspecto de lo dibujado.
- [[05 Transformaciones]] — `translate` y `rotate` para posicionar formas sin cambiar sus coordenadas internas.
- [[index|Canvas API]] — visión general.
