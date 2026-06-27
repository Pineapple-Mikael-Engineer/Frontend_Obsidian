---
title: WeakMap y WeakSet — Colecciones con referencias débiles
aliases:
  - WeakMap
  - WeakSet
  - referencias débiles JavaScript
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: WeakMap
tipo: clase
draft: false
order: 4
---

# WeakMap y WeakSet

> [!definicion]
> `WeakMap` y `WeakSet` son colecciones donde las referencias a sus claves (WeakMap) o valores (WeakSet) son **débiles**: si no hay otras referencias al objeto fuera de la colección, el recolector de basura puede eliminarlo y la entrada desaparece automáticamente. Como consecuencia, no son iterables, no tienen `size`, y sus claves/valores deben ser objetos (o Symbols registrados globalmente). Son herramientas de gestión de memoria, no de almacenamiento general.

```js
let obj = { nombre: "temporal" };
const wm = new WeakMap();
wm.set(obj, { meta: "datos extra" });

console.log(wm.has(obj)); // → true

obj = null; // la única referencia fuerte desaparece
// en algún momento el GC recolecta obj y wm elimina la entrada
```

## Comparativa con Map y Set

| Característica | Map | Set | WeakMap | WeakSet |
|----------------|-----|-----|---------|---------|
| Tipo de clave/valor | cualquier tipo | cualquier tipo | solo objetos | solo objetos |
| Iterable | sí | sí | no | no |
| Tiene `size` | sí | sí | no | no |
| Tiene `forEach` | sí | sí | no | no |
| Impide GC de claves | sí | sí | no | no |
| Permite GC automático | no | no | sí | sí |
| Métodos disponibles | set/get/has/delete/clear + iteradores | add/has/delete/clear + iteradores | get/set/has/delete | add/has/delete |

## API de WeakMap

```js
const wm = new WeakMap();
wm.set(clave, valor)  // → el propio WeakMap
wm.get(clave)         // → valor o undefined
wm.has(clave)         // → boolean
wm.delete(clave)      // → boolean
```

## API de WeakSet

```js
const ws = new WeakSet();
ws.add(objeto)        // → el propio WeakSet
ws.has(objeto)        // → boolean
ws.delete(objeto)     // → boolean
```

## Mecanismo interno

El motor mantiene un registro separado de referencias débiles. Cuando el GC realiza un ciclo de recolección, si detecta que un objeto clave de un WeakMap o WeakSet solo es referenciado desde esa colección (sin referencias fuertes en ningún otro lugar), lo marca para eliminación. La entrada del WeakMap/WeakSet desaparece en ese ciclo. La no-iterabilidad es una consecuencia directa: en cualquier punto entre dos instrucciones de código, el GC podría haber eliminado entradas, haciendo imposible ofrecer una vista estable de los contenidos.

## Notas relacionadas

- [[01 Claves Débiles y Garbage Collection]]
- [[02 Casos de Uso]]
- [[03 Set/index|Set — índice]]
- [[02 Map/index|Map — índice]]
