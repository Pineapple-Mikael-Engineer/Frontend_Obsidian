---
title: CustomEvent y dispatchEvent — Crear y disparar eventos personalizados
aliases:
  - CustomEvent
  - dispatchEvent
  - new CustomEvent
tags:
  - javascript
  - api/clase
  - eventos
objeto: EventTarget
tipo: clase
retorna: CustomEvent
muta: false
asincrono: false
draft: false
---

# CustomEvent y dispatchEvent

> [!definicion]
> `new CustomEvent(tipo, opciones?)` crea un objeto de evento con un nombre arbitrario. `elemento.dispatchEvent(evento)` lo dispara en ese elemento, propagándolo por el árbol DOM como cualquier evento nativo. El dispatch es **síncrono**: todos los listeners se ejecutan antes de que `dispatchEvent` retorne.

```js
// Crear
const ev = new CustomEvent('stock:agotado', {
  bubbles: true,
  cancelable: false,
  detail: { producto: 'SKU-001' },
});

// Disparar
tarjetaProducto.dispatchEvent(ev);
// → los listeners de 'stock:agotado' en tarjetaProducto y sus ancestros se ejecutan aquí
// → dispatchEvent retorna true (o false si se llamó preventDefault y era cancelable)
```

## Constructor: `new CustomEvent(tipo, opciones)`

| Parámetro | Tipo | Descripción |
|---|---|---|
| `tipo` | string | Nombre del evento. Convención: `'dominio:accion'` o `'kebab-case'` |
| `opciones.bubbles` | boolean | Default `false`. Si `true`, el evento sube por el árbol |
| `opciones.cancelable` | boolean | Default `false`. Si `true`, `preventDefault()` tiene efecto |
| `opciones.detail` | any | Datos arbitrarios accesibles en `e.detail` |

## `dispatchEvent`

`elemento.dispatchEvent(evento)` retorna `true` si el evento no fue cancelado (o si `cancelable` es `false`); retorna `false` si se llamó `e.preventDefault()` y el evento era cancelable.

No se puede re-despachar un evento que ya fue despachado — el intento silenciosamente no tiene efecto (o lanza error en modo estricto de la implementación).

## Escucha del evento

La escucha es idéntica a cualquier evento nativo:

```js
// Escuchar en el emisor
tarjetaProducto.addEventListener('stock:agotado', (e) => {
  console.log(e.detail.producto);
});

// Escuchar en un ancestro (si bubbles: true)
document.addEventListener('stock:agotado', (e) => {
  notificarInventario(e.detail);
});
```

## Receta: componente que emite un evento al cambiar estado

```js
class Slider {
  constructor(el) {
    this.el = el;
    this.valor = 0;

    el.addEventListener('input', (e) => {
      this.valor = Number(e.target.value);
      this.emitirCambio();
    });
  }

  emitirCambio() {
    this.el.dispatchEvent(new CustomEvent('slider:cambio', {
      bubbles: true,
      detail: { valor: this.valor },
    }));
  }
}

// Desde fuera:
document.addEventListener('slider:cambio', (e) => {
  actualizarVistaPrevia(e.detail.valor);
});
```

## Sincronicidad

`dispatchEvent` es síncrono: el flujo de ejecución entra en los listeners, los completa, y solo entonces retorna. No hay microtask queue de por medio. Esto difiere de los eventos del navegador (como `click` generado por el usuario), que se procesan en la cola de eventos del event loop.

```js
console.log('antes');
el.dispatchEvent(new CustomEvent('test'));
// los listeners de 'test' se ejecutan aquí, de forma síncrona
console.log('después');  // → siempre imprime después de los listeners
```

> [!tip]
> Para el tipo del evento, usar convenciones como `'componente:accion'` (`'modal:cerrado'`, `'formulario:enviado'`) o `'kebab-case'` (`'datos-cargados'`). Evitar tipos genéricos como `'change'` o `'update'` que podrían colisionar con eventos nativos.

> [!warning]
> Con `bubbles: false` (el default), el evento no sube — solo los listeners registrados directamente en el elemento emisor lo reciben. Para comunicación hijo→padre o hacia `document`, es necesario `bubbles: true`.

## Notas relacionadas

- [[02 Pasar Datos con detail|Pasar Datos con detail]]
- [[06 Eventos Personalizados/index|Eventos Personalizados]]
- [[04 Fases del Evento/index|Fases del Evento]]
- [[01 addEventListener|addEventListener]]
