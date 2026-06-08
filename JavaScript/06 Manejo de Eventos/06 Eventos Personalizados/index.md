---
title: Eventos Personalizados — Índice
aliases:
  - CustomEvent
  - eventos custom
  - custom events
tags:
  - javascript
  - api/concepto
  - eventos
draft: false
---

# Eventos Personalizados

> [!definicion]
> Los eventos personalizados permiten comunicar partes de una aplicación reutilizando el sistema de eventos del DOM. Un componente emite un `CustomEvent` con `dispatchEvent`; cualquier ancestro (u otro oyente) escucha ese evento con `addEventListener`. El emisor y el receptor no necesitan referencia directa el uno al otro — el desacoplamiento es estructural.

```js
// Emisor (componente hijo)
const evento = new CustomEvent('pedido:completado', {
  bubbles: true,
  detail: { id: 42, total: 99.99 },
});
botonPagar.dispatchEvent(evento);

// Receptor (ancestro o document)
document.addEventListener('pedido:completado', (e) => {
  actualizarCarrito(e.detail);
});
```

## Subsecciones

| # | Nota | Cubre |
|---|---|---|
| 01 | CustomEvent y dispatchEvent | Constructor, opciones, `dispatchEvent`, sincronicidad |
| 02 | Pasar Datos con detail | La propiedad `detail`, patrones de comunicación entre componentes |

## Cuándo usar eventos personalizados

Cuando la comunicación es de hijo a padre (o a cualquier ancestro) y se prefiere desacoplamiento sobre la simplicidad de un callback prop. El evento sube por el árbol DOM naturalmente con `bubbles: true`, sin necesidad de pasar funciones como props o mantener un bus de eventos manual.

## Notas relacionadas

- [[01 CustomEvent y dispatchEvent|CustomEvent y dispatchEvent]]
- [[02 Pasar Datos con detail|Pasar Datos con detail]]
- [[04 Fases del Evento/index|Fases del Evento]]
- [[05 Delegación de Eventos|Delegación de Eventos]]
