---
title: Actualizaciones Eficientes del DOM
aliases:
  - actualizaciones eficientes
  - DOM performance
  - optimización DOM
tags:
  - javascript
  - api/concepto
  - dom
draft: false
order: 9
---

# Actualizaciones Eficientes del DOM

> [!definicion]
> Cada modificación del DOM puede desencadenar un **reflow** (recálculo de layout) y un **repaint** (redibujado de píxeles), operaciones que bloquean el hilo principal. Las técnicas de actualización eficiente buscan reducir el número y el coste de estos ciclos: agrupar inserciones con `DocumentFragment`, sincronizar animaciones con `requestAnimationFrame`, y evitar el layout thrashing separando lecturas de escrituras.

El pipeline de renderizado del navegador sigue el orden: **JavaScript → Style → Layout → Paint → Composite**. Cada etapa puede omitirse si los cambios no la requieren. Las optimizaciones apuntan a reducir qué etapas se ejecutan y con qué frecuencia.

## Las tres técnicas

### `DocumentFragment` — insertar en lotes

Nodo virtual que vive fuera del DOM. Se construye en memoria y se inserta en una sola operación, causando un único reflow en lugar de uno por cada nodo hijo.

```js
const frag = document.createDocumentFragment();
datos.forEach(d => {
  const li = document.createElement('li');
  li.textContent = d.nombre;
  frag.appendChild(li);
});
lista.appendChild(frag); // un solo reflow
```

Ver [[01 DocumentFragment]].

### `requestAnimationFrame` — sincronizar con el renderizado

Programa la ejecución de un callback justo antes del próximo repaint. Las animaciones gestionadas con rAF están sincronizadas con el ciclo de 60fps del monitor; las gestionadas con `setTimeout` no lo están.

```js
function animar(ts) {
  el.style.transform = `translateX(${calcularPosicion(ts)}px)`;
  requestAnimationFrame(animar);
}
requestAnimationFrame(animar);
```

Ver [[02 requestAnimationFrame]].

### Reflows y Repaints — separar lecturas de escrituras

Leer una propiedad geométrica inmediatamente después de escribir un estilo fuerza un reflow síncrono (layout thrashing). La regla es: **todas las lecturas primero, luego todas las escrituras**.

```js
// MAL
elements.forEach(el => { el.style.width = el.offsetWidth + 10 + 'px'; });

// BIEN
const anchos = elements.map(el => el.offsetWidth);
elements.forEach((el, i) => { el.style.width = anchos[i] + 10 + 'px'; });
```

Ver [[03 Reflows y Repaints]].

## Tabla de estrategias

| Problema | Técnica | Beneficio |
|---|---|---|
| Insertar N nodos en bucle | `DocumentFragment` o `el.append(...nodos)` | Un solo reflow |
| Animar propiedades visuales | `requestAnimationFrame` | Sincronizado con el monitor |
| Leer y escribir layout en bucle | Separar lecturas y escrituras | Elimina reflows síncronos |
| Detectar visibilidad de elementos | `IntersectionObserver` | Sin polling ni reflows |
| Animar sin afectar layout | `transform` y `opacity` | Solo composite, sin reflow ni repaint |

## Notas relacionadas

- [[01 DocumentFragment]]
- [[02 requestAnimationFrame]]
- [[03 Reflows y Repaints]]
