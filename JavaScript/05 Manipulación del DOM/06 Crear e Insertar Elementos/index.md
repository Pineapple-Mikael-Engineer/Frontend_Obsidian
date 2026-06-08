---
title: "Crear e Insertar Elementos — Índice"
aliases:
  - Índice Crear e Insertar Elementos
  - DOM inserción JS
tags:
  - javascript
  - dom
  - index
draft: false
---

# Crear e Insertar Elementos

El flujo de trabajo estándar para añadir contenido dinámico al DOM tiene tres pasos: **crear** el elemento con `createElement` → **configurar** sus atributos, clases y contenido → **insertar** en el árbol con un método de inserción.

```js
// Flujo típico
const li = document.createElement("li");   // 1. crear
li.textContent = "Nuevo item";             // 2. configurar
li.classList.add("item");
lista.appendChild(li);                     // 3. insertar
```

La elección del método de inserción depende de la posición deseada respecto al nodo de referencia:

| Método | Se invoca sobre | Posición de inserción | Acepta strings | API |
|---|---|---|---|---|
| `appendChild(nodo)` | padre | Último hijo | No | Clásica |
| `append(...items)` | padre | Último hijo | Sí | Moderna |
| `prepend(...items)` | padre | Primer hijo | Sí | Moderna |
| `insertBefore(nuevo, ref)` | padre | Antes de `ref` | No | Clásica |
| `before(...items)` | nodo de referencia | Antes del nodo (hermano) | Sí | Moderna |
| `after(...items)` | nodo de referencia | Después del nodo (hermano) | Sí | Moderna |
| `replaceWith(...items)` | nodo a reemplazar | En lugar del nodo | Sí | Moderna |

Los métodos de API moderna (`append`, `prepend`, `before`, `after`, `replaceWith`) son más ergonómicos: aceptan múltiples argumentos, admiten strings directos (convertidos a nodos de texto) y no requieren referencias adicionales. Los clásicos (`appendChild`, `insertBefore`) son útiles por compatibilidad o cuando se necesita el valor de retorno.

## Notas relacionadas

- [[01 createElement y createTextNode]]
- [[02 appendChild y append]]
- [[03 prepend]]
- [[04 insertBefore]]
- [[05 before y after]]
- [[06 cloneNode]]
- [[07 replaceChild y replaceWith]]
