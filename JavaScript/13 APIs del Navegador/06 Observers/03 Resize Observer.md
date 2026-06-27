---
title: ResizeObserver — detectar cambios de tamaño en elementos
aliases:
  - ResizeObserver
  - Resize Observer
tags:
  - javascript
  - api/clase
  - browser
objeto: Web API
tipo: clase
retorna: ResizeObserver
asincrono: true
draft: false
order: 3
---

# ResizeObserver

> [!definicion]
> `ResizeObserver` detecta cambios en el tamaño de elementos individuales del DOM y entrega notificaciones asíncronas antes del paint. A diferencia de `window.addEventListener("resize", ...)`, observa cualquier elemento (no solo la ventana), lo que lo convierte en el mecanismo correcto para componentes que necesitan reaccionar a su propio tamaño. El callback recibe un array de `ResizeObserverEntry` con las nuevas dimensiones.

```js
const observer = new ResizeObserver(entries => {
  entries.forEach(entry => {
    const { width, height } = entry.contentRect;
    ajustarLayout(entry.target, width, height);
  });
});

observer.observe(elemento);
observer.observe(otroElemento);  // puede observar múltiples elementos

observer.unobserve(elemento);    // dejar de observar uno
observer.disconnect();           // dejar de observar todos
```

## Propiedades de `ResizeObserverEntry`

| Propiedad | Tipo | Descripción |
|---|---|---|
| `target` | `Element` | El elemento cuyo tamaño cambió |
| `contentRect` | `DOMRectReadOnly` | Dimensiones del content box (sin padding ni border) — la forma más directa de obtener ancho/alto |
| `contentBoxSize` | `ResizeObserverSize[]` | Array de tamaños del content box (`inlineSize`, `blockSize`) |
| `borderBoxSize` | `ResizeObserverSize[]` | Array de tamaños del border box (incluye padding y border) |
| `devicePixelContentBoxSize` | `ResizeObserverSize[]` | Tamaño en píxeles físicos del dispositivo (relevante para canvas HiDPI) |

`ResizeObserverSize` expone `inlineSize` (ancho en escritura horizontal) y `blockSize` (alto en escritura horizontal).

## Opciones de `observe()`

```js
// Por defecto observa el content-box
observer.observe(elemento);

// Observar el border-box (incluye padding y border)
observer.observe(elemento, { box: "border-box" });

// Observar en píxeles de dispositivo (para canvas)
observer.observe(canvas, { box: "device-pixel-content-box" });
```

| Valor de `box` | Qué mide |
|---|---|
| `"content-box"` (default) | Solo el área de contenido, sin padding ni border |
| `"border-box"` | Contenido + padding + border |
| `"device-pixel-content-box"` | Área de contenido en píxeles físicos del dispositivo |

## Casos de uso

**Gráficos responsivos (charts)** — observar el contenedor del chart; al cambiar su tamaño, re-renderizar con las nuevas dimensiones. Evita el patrón frágil de escuchar `window.resize` y asumir que el contenedor cambió proporcionalmente.

```js
const chart = crearChart(contenedor);
const observer = new ResizeObserver(entries => {
  const { width, height } = entries[0].contentRect;
  chart.resize(width, height);
});
observer.observe(contenedor);
```

**Canvas HiDPI** — `box: "device-pixel-content-box"` proporciona el tamaño exacto en píxeles físicos, necesario para escalar correctamente el bitmap del canvas en pantallas Retina sin leer `devicePixelRatio` manualmente.

**Editores de texto y toolbars adaptativos** — ajustar el número de botones visibles en un toolbar según el ancho disponible del contenedor (patrón "priority+ toolbar").

**Container queries en JS** — antes de que CSS Container Queries tuviera soporte universal, `ResizeObserver` era el mecanismo estándar para aplicar estilos o clases condicionalmente según el tamaño del contenedor, no del viewport.

## ResizeObserver vs CSS Container Queries

| Criterio | ResizeObserver | CSS Container Queries |
|---|---|---|
| Lenguaje | JavaScript | CSS puro |
| Rendimiento | Bueno (asíncrono, antes del paint) | Mejor (manejado por el motor de layout) |
| Flexibilidad | Total (puede ejecutar cualquier lógica JS) | Solo aplicar estilos CSS |
| Soporte | Universal (todos los evergreen browsers) | Soporte amplio desde 2023 |
| Uso recomendado | Lógica JS dependiente del tamaño | Estilos adaptativos puramente CSS |

## Cómo funciona por dentro

`ResizeObserver` entrega sus callbacks en la fase de render del event loop, después de que el layout ha sido calculado y antes de que el paint ocurra. Esto garantiza que las dimensiones reportadas son las definitivas para ese frame. El callback se procesa en el orden del árbol DOM (elementos padre antes que hijos) para garantizar que los ajustes de un padre no invaliden los de un hijo.

> [!tip]
> El primer disparo del callback ocurre después del primer `observe()`, entregando el tamaño inicial del elemento. Esto elimina la necesidad de leer manualmente el tamaño al montar el componente.

> [!warning]
> Si el callback de `ResizeObserver` modifica el tamaño del elemento que está siendo observado (por ejemplo, cambiando su `height` en respuesta al evento), puede crearse un bucle infinito. Los browsers lanzan una advertencia `ResizeObserver loop limit exceeded` y cortan el ciclo, pero el comportamiento resulta en callbacks que se descartan silenciosamente. La solución es asegurarse de que los cambios de tamaño realizados dentro del callback no afecten al elemento observado, o usar `requestAnimationFrame` para diferir la modificación.

## Notas relacionadas

- [[index|Observers]] — comparativa de los tres observers y criterios de elección.
- [[01 Intersection Observer]] — para reaccionar a visibilidad, no a tamaño.
- [[../05 Canvas API/index|Canvas API]] — `ResizeObserver` con `device-pixel-content-box` para adaptar el canvas a pantallas HiDPI.
