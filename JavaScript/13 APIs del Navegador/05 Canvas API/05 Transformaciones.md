---
title: Transformaciones — translate, rotate, scale, save/restore
aliases:
  - canvas transformaciones
  - ctx.save
  - ctx.restore
  - canvas animate
  - translate rotate scale canvas
tags:
  - javascript
  - api/objeto
  - canvas
objeto: CanvasRenderingContext2D
tipo: concepto
draft: false
---

# Transformaciones

> [!definicion]
> Las transformaciones del contexto 2D modifican el **sistema de coordenadas** sobre el que se dibujan los elementos posteriores. Se implementan internamente como una matriz de transformación afín 2D (3×3 en coordenadas homogéneas). `translate`, `rotate` y `scale` acumulan transformaciones sobre la actual; `setTransform` la reemplaza. `save()`/`restore()` apilan y desapilan el estado completo del contexto (transformación, estilos y clip) para aislar grupos de dibujo.

```js
ctx.save();
ctx.translate(200, 200); // mover el origen al centro del canvas
ctx.rotate(Math.PI / 4); // 45° en sentido horario
ctx.fillStyle = "#4f46e5";
ctx.fillRect(-50, -50, 100, 100); // cuadrado centrado en el nuevo origen
ctx.restore();
```

## Métodos de transformación

| Método | Descripción |
|---|---|
| `ctx.translate(x, y)` | Desplaza el origen del sistema de coordenadas en (x, y) |
| `ctx.rotate(angulo)` | Rota el sistema alrededor del origen actual. Positivo = horario |
| `ctx.scale(sx, sy)` | Escala el sistema. `scale(-1, 1)` = espejo horizontal; `scale(1, -1)` = espejo vertical |
| `ctx.transform(a, b, c, d, e, f)` | Multiplica la transformación actual por la matriz especificada |
| `ctx.setTransform(a, b, c, d, e, f)` | Reemplaza la transformación actual por la matriz especificada |
| `ctx.resetTransform()` | Restablece la transformación a la identidad (sin efectos) |

### Parámetros de la matriz (a, b, c, d, e, f)

La matriz de transformación afín 2D es:

```
[ a  c  e ]
[ b  d  f ]
[ 0  0  1 ]
```

- `a`, `d`: escala horizontal/vertical.
- `b`, `c`: cizallamiento (shear).
- `e`, `f`: traslación en x/y.

La identidad es `(1, 0, 0, 1, 0, 0)`.

## Estado del contexto: save y restore

`ctx.save()` apila una copia completa del estado del contexto sobre una pila interna. `ctx.restore()` desapila el último estado guardado y lo aplica. El estado incluye:

- Transformación actual (la matriz completa).
- Todas las propiedades de estilo (`fillStyle`, `strokeStyle`, `lineWidth`, `font`, `globalAlpha`, sombras, `globalCompositeOperation`, `lineCap`, `lineJoin`, `setLineDash`…).
- El clipping path actual.

**La pila no tiene límite de profundidad en la especificación**, aunque en la práctica cada `save` sin su `restore` correspondiente filtra memoria. El patrón correcto es siempre emparejar `save` y `restore`.

```js
ctx.save();
  // estado A: modificaciones locales
  ctx.translate(100, 100);
  ctx.fillStyle = "red";
  ctx.fillRect(0, 0, 50, 50);

  ctx.save();
    // estado B: más modificaciones
    ctx.rotate(0.5);
    ctx.fillStyle = "blue";
    ctx.fillRect(0, 0, 30, 30);
  ctx.restore(); // vuelve al estado A

ctx.restore(); // vuelve al estado anterior al primer save
```

## Receta: rueda girada (translate + rotate + save/restore)

```js
function dibujarRueda(ctx, cx, cy, radio, dientes, angulo) {
  ctx.save();
  ctx.translate(cx, cy);
  ctx.rotate(angulo);

  // Cuerpo de la rueda
  ctx.beginPath();
  ctx.arc(0, 0, radio, 0, Math.PI * 2);
  ctx.fillStyle = "#64748b";
  ctx.fill();

  // Dientes
  ctx.fillStyle = "#334155";
  for (let i = 0; i < dientes; i++) {
    ctx.save();
    ctx.rotate((i / dientes) * Math.PI * 2);
    ctx.fillRect(radio - 5, -4, 14, 8); // diente centrado en el eje X
    ctx.restore();
  }

  // Orificio central
  ctx.beginPath();
  ctx.arc(0, 0, radio * 0.25, 0, Math.PI * 2);
  ctx.fillStyle = "#f1f5f9";
  ctx.fill();

  ctx.restore();
}

dibujarRueda(ctx, 200, 200, 70, 12, 0);
```

## Receta: animación con requestAnimationFrame

```js
let angulo = 0;

function frame() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  dibujarRueda(ctx, 200, 200, 70, 12, angulo);
  angulo += 0.02; // ~1.1°/frame a 60fps

  requestAnimationFrame(frame);
}

requestAnimationFrame(frame);
```

El patrón es siempre: `clearRect` → recalcular estado → redibujar todo. El canvas es un bitmap; no hay "mover" un objeto. Para detener la animación, guardar el id retornado por `requestAnimationFrame` y cancelar con `cancelAnimationFrame(id)`.

## Orden de las transformaciones

Las transformaciones se acumulan y el orden importa: `translate` seguido de `rotate` produce un resultado distinto a `rotate` seguido de `translate`.

```js
// (A) Trasladar primero, luego rotar → la rotación ocurre alrededor del punto trasladado
ctx.translate(100, 100);
ctx.rotate(Math.PI / 4);
ctx.fillRect(0, 0, 50, 50); // cuadrado girado 45° alrededor de (100,100)

// (B) Rotar primero, luego trasladar → la traslación ocurre a lo largo del eje rotado
ctx.rotate(Math.PI / 4);
ctx.translate(100, 100);
ctx.fillRect(0, 0, 50, 50); // cuadrado trasladado en dirección del eje girado
```

La regla práctica: si se quiere rotar un objeto alrededor de su propio centro, usar `translate` al centro → `rotate` → dibujar centrado en (0,0).

## Cómo funciona por dentro

El contexto mantiene una única matriz de transformación 3×3 (en coordenadas homogéneas). Cada llamada a `translate`, `rotate` o `scale` premultiplica esa matriz por la transformación especificada. Cuando se llama a `fillRect`, `drawImage`, `fillText` o cualquier operación de dibujo, las coordenadas se transforman con esa matriz antes de escribir los píxeles en el bitmap.

`save` y `restore` operan sobre una pila de matrices (junto con el resto del estado): son la forma estándar de aislar transformaciones locales sin necesidad de calcular transformaciones inversas.

> [!warning]
> `ctx.rotate` toma el ángulo en **radianes**, no en grados. Para convertir: `radianes = grados * (Math.PI / 180)`. Un ángulo positivo rota en sentido **horario** (al contrario de la convención matemática estándar, porque el eje Y del canvas apunta hacia abajo).

> [!tip]
> Para animar a 60 fps de forma eficiente, pasar el timestamp recibido por `requestAnimationFrame` al callback y calcular el delta respecto al frame anterior: `const delta = timestamp - prevTimestamp`. Así la velocidad de la animación es independiente de la frecuencia de refresco del monitor.

## Notas relacionadas

- [[01 Contexto 2D]] — el estado del contexto que `save`/`restore` preserva, y el sistema de coordenadas base.
- [[02 Dibujar Formas y Trazados]] — las formas que reciben las transformaciones activas.
- [[03 Estilos (fillStyle, strokeStyle)]] — los estilos también forman parte del estado guardado por `save`.
- [[index|Canvas API]] — visión general.
