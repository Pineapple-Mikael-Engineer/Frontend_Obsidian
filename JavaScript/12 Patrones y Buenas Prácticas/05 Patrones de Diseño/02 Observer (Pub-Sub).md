---
title: Observer (Pub-Sub)
aliases:
  - Observer pattern
  - Pub/Sub
  - publicar suscribir
  - EventEmitter
tags:
  - javascript
  - api/concepto
  - patrones
draft: false
order: 2
---

# Observer (Pub-Sub)

> [!definicion]
> El patrón **Observer** (también llamado **Pub/Sub** — publicar/suscribir) permite que objetos **suscriptores** reciban notificaciones automáticas cuando el estado de otro objeto **sujeto** cambia. Desacopla emisores y receptores: el emisor no necesita conocer ni gestionar a sus suscriptores; solo anuncia que algo ocurrió.

## EventEmitter — implementación canónica

```js
class EventEmitter {
  #listeners = new Map();

  on(evento, handler) {
    if (!this.#listeners.has(evento)) this.#listeners.set(evento, []);
    this.#listeners.get(evento).push(handler);
    return this; // permite chaining: emitter.on("a", f).on("b", g)
  }

  off(evento, handler) {
    const handlers = this.#listeners.get(evento);
    if (handlers) {
      this.#listeners.set(evento, handlers.filter(h => h !== handler));
    }
    return this;
  }

  emit(evento, ...args) {
    this.#listeners.get(evento)?.forEach(h => h(...args));
    return this;
  }

  once(evento, handler) {
    const wrapper = (...args) => {
      handler(...args);
      this.off(evento, wrapper);
    };
    return this.on(evento, wrapper);
  }
}
```

`once` registra un handler que se desuscribe automáticamente tras la primera ejecución, usando un wrapper que llama a `off` con la referencia al propio wrapper.

## Uso básico

```js
const bus = new EventEmitter();

function onLogin(usuario) {
  console.log(`Bienvenido, ${usuario.nombre}`);
}

bus.on("login", onLogin);
bus.on("login", usuario => actualizarUI(usuario));

bus.emit("login", { nombre: "Ana", rol: "admin" });
// → "Bienvenido, Ana"
// → actualiza la UI

bus.off("login", onLogin); // desuscribir un handler específico

bus.once("primer-clic", () => mostrarOnboarding());
```

## Observer vs Pub/Sub estricto

| Dimensión | Observer | Pub/Sub |
|---|---|---|
| Intermediario | No — el sujeto gestiona a sus observadores | Sí — un bus o broker central |
| Acoplamiento | Débil: el sujeto conoce la interfaz del observador | Casi nulo: emisores y receptores no se conocen |
| Ejemplo nativo | DOM `addEventListener` — el nodo conoce sus listeners | `BroadcastChannel`, `window.postMessage` |
| Ejemplo de lib | Node.js `EventEmitter` | Redux store, RxJS Subject |

La implementación anterior es técnicamente un Observer con interfaz Pub/Sub: el `EventEmitter` hace de intermediario débil.

## Casos de uso

| Situación | Por qué Observer |
|---|---|
| Sistema de notificaciones de la app | Cualquier módulo puede escuchar sin conocer la fuente |
| Sincronización entre componentes no relacionados | Evita pasar callbacks en cascada por props |
| Plugin system | Los plugins se suscriben a hooks del ciclo de vida |
| WebSockets de alto nivel | El cliente emite eventos tipados desde mensajes crudos |
| Formularios con validación reactiva | Cada campo emite `cambio`; el validador escucha |

## Cómo funciona por dentro

El `EventEmitter` mantiene un `Map<string, Function[]>` donde cada clave es el nombre del evento y el valor es un array de handlers. `emit` itera el array en orden de registro y llama a cada handler con los argumentos proporcionados. Los handlers se ejecutan de forma **síncrona** y en orden de suscripción.

La relación con el sistema de eventos del DOM es directa: `addEventListener` / `dispatchEvent` son la implementación nativa del patrón Observer integrada en el browser. `CustomEvent` permite emitir eventos con payload arbitrario sobre cualquier nodo del DOM.

```js
// Observer nativo del DOM
const boton = document.querySelector("#btn");
boton.addEventListener("click", handler);     // on
boton.removeEventListener("click", handler);  // off

// CustomEvent como Pub/Sub sobre el DOM
document.addEventListener("app:login", e => console.log(e.detail));
document.dispatchEvent(new CustomEvent("app:login", { detail: { nombre: "Ana" } }));
```

> [!tip]
> Para aplicaciones sin framework que necesitan comunicación entre componentes, un `EventEmitter` singleton exportado como módulo actúa como bus de eventos global ligero y sin dependencias externas. Es el núcleo de arquitecturas como los stores de Flux antes de Redux.

> [!warning]
> Los handlers registrados con `on` mantienen referencias vivas a sus closures. Si un componente se destruye sin llamar a `off`, el handler persiste en el array del emisor y provoca **memory leaks** y ejecuciones en objetos ya desmontados. La práctica obligatoria: desregistrar en el ciclo de limpieza del componente (`componentWillUnmount`, `disconnectedCallback`, `onDestroy`, etc.). Para listeners de un solo uso, `once` es la alternativa que se limpia sola.

## Notas relacionadas

- [[01 Singleton|Singleton]]
- [[05 Patrones de Diseño/index|Patrones de Diseño]]
