---
title: MutationObserver — observar cambios en el árbol del DOM
aliases:
  - MutationObserver
  - Mutation Observer
tags:
  - javascript
  - api/clase
  - browser
objeto: Web API
tipo: clase
retorna: MutationObserver
asincrono: true
draft: false
order: 2
---

# MutationObserver

> [!definicion]
> `MutationObserver` observa cambios en el árbol del DOM: nodos añadidos o eliminados, atributos modificados y cambios en el contenido de nodos de texto. El callback recibe un array de `MutationRecord` agrupados en lote, ejecutado como microtask después del script actual y antes del render. Es la API moderna que reemplaza al deprecado `MutationEvent`.

```js
const observer = new MutationObserver(mutations => {
  mutations.forEach(mutation => {
    if (mutation.type === "childList") {
      mutation.addedNodes.forEach(node => console.log("añadido:", node));
      mutation.removedNodes.forEach(node => console.log("eliminado:", node));
    }
    if (mutation.type === "attributes") {
      console.log(`atributo "${mutation.attributeName}" cambió en`, mutation.target);
    }
  });
});

observer.observe(document.getElementById("contenedor"), {
  childList: true,
  subtree: true,
  attributes: true,
  attributeFilter: ["class", "data-estado"],
  characterData: false
});

observer.disconnect(); // dejar de observar
```

## Opciones de `observe()`

| Opción | Tipo | Qué observa |
|---|---|---|
| `childList` | `boolean` | Nodos hijos añadidos o eliminados directamente bajo el target |
| `subtree` | `boolean` | Extender la observación a todos los descendientes del target |
| `attributes` | `boolean` | Cambios en atributos del target (o del subárbol si `subtree: true`) |
| `attributeFilter` | `string[]` | Lista blanca de atributos a observar (si se omite, observa todos) |
| `attributeOldValue` | `boolean` | Guardar el valor anterior del atributo en `mutation.oldValue` |
| `characterData` | `boolean` | Cambios en el contenido de nodos `Text` |
| `characterDataOldValue` | `boolean` | Guardar el texto anterior en `mutation.oldValue` |

Al menos una de `childList`, `attributes` o `characterData` debe ser `true`; de lo contrario se lanza `TypeError`.

## Propiedades de `MutationRecord`

| Propiedad | Tipo | Descripción |
|---|---|---|
| `type` | `"childList"` \| `"attributes"` \| `"characterData"` | Tipo de mutación |
| `target` | `Node` | Nodo que fue mutado |
| `addedNodes` | `NodeList` | Nodos añadidos (vacío si no aplica) |
| `removedNodes` | `NodeList` | Nodos eliminados (vacío si no aplica) |
| `attributeName` | `string` \| `null` | Nombre del atributo modificado |
| `oldValue` | `string` \| `null` | Valor anterior (solo si se pidió `attributeOldValue` o `characterDataOldValue`) |
| `previousSibling` | `Node` \| `null` | Hermano anterior al nodo añadido/eliminado |
| `nextSibling` | `Node` \| `null` | Hermano siguiente al nodo añadido/eliminado |

## Métodos del observer

```js
observer.observe(target, opciones);  // iniciar observación
observer.disconnect();               // detener todas las observaciones
const pendientes = observer.takeRecords(); // vaciar la cola de mutaciones pendientes sin invocar el callback
```

## Casos de uso

**Detectar cuando un framework inserta un componente** — observar el nodo raíz de la app con `childList: true, subtree: true` para reaccionar cuando se añade un elemento específico al DOM.

**Sincronizar con cambios de atributos de terceros** — una librería externa modifica `data-estado` de un elemento; `attributeFilter: ["data-estado"]` notifica exactamente esos cambios sin coste extra.

**Extensiones de browser** — observar cambios en páginas de terceros para aplicar transformaciones sobre contenido dinámico que no se puede capturar con un simple `DOMContentLoaded`.

**Reactividad básica sin framework** — observar atributos de un Custom Element para actualizar la UI en respuesta a cambios externos sin necesidad de un store centralizado.

## Cómo funciona por dentro

Las mutaciones se acumulan internamente durante la ejecución del script actual. Al terminar el script (al final del task actual), el callback se encola como **microtask** — lo que significa que se ejecuta antes del próximo render, pero después de que el stack de llamadas sincrónico esté vacío. Este comportamiento lo hace más eficiente que los deprecados `MutationEvent`, que se disparaban síncronamente en cada mutación y podían causar reflows encadenados.

```js
const observer = new MutationObserver(mutations => {
  console.log("2 — microtask: mutations batch"); // ejecuta segundo
});
observer.observe(document.body, { childList: true });

document.body.appendChild(document.createElement("div"));
console.log("1 — síncrono"); // ejecuta primero
// "1 — síncrono"
// "2 — microtask: mutations batch"
```

> [!tip]
> `observer.takeRecords()` vacía la cola interna y retorna los `MutationRecord` pendientes sin invocar el callback. Es útil cuando se necesita procesar las mutaciones restantes antes de llamar a `disconnect()`, por ejemplo al desmontar un componente.

> [!warning]
> Modificar el DOM dentro del callback de un `MutationObserver` puede disparar nuevas mutaciones que volverán a invocar el callback. El browser detecta y rompe los ciclos obvios, pero la lógica de guardas explícitas (`if (procesando) return`) es responsabilidad del desarrollador para evitar bucles infinitos.

## Notas relacionadas

- [[index|Observers]] — comparativa de los tres observers.
- [[01 Intersection Observer]] — para reaccionar a visibilidad, no a cambios estructurales.
- [[../../08 Comunicación con APIs/index|Comunicación con APIs]] — patrón complementario: polling vs observer para detectar actualizaciones de datos.
