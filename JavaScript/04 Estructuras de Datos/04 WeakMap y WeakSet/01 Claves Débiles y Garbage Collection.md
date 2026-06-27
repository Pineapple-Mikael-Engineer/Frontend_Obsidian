---
title: WeakMap y WeakSet — Claves Débiles y Garbage Collection
aliases:
  - referencias débiles GC
  - WeakMap garbage collection
  - weak references javascript
tags:
  - javascript
  - api/clase
  - estructuras-datos
objeto: WeakMap
tipo: concepto
draft: false
order: 1
---

# WeakMap y WeakSet — Claves Débiles y Garbage Collection

> [!definicion]
> En un `Map` o `Set` normal, la referencia a un objeto-clave es **fuerte**: el GC no puede recolectar el objeto mientras esté en la colección. En un `WeakMap` o `WeakSet`, la referencia es **débil**: el GC puede recolectar el objeto si no existen otras referencias fuertes hacia él, y la entrada de la colección desaparece automáticamente. Esta propiedad hace que WeakMap y WeakSet no sean iterables ni tengan `size`.

```js
// Map fuerte — impide GC
let elemento = document.getElementById("btn");
const mapaFuerte = new Map();
mapaFuerte.set(elemento, { clicks: 0 });

elemento = null; // la variable local ya no apunta al nodo
// pero mapaFuerte aún tiene una referencia fuerte: el nodo NO es recolectado
// → memory leak si el nodo fue eliminado del DOM

// WeakMap débil — no impide GC
let elemento2 = document.getElementById("btn2");
const mapaDebil = new WeakMap();
mapaDebil.set(elemento2, { clicks: 0 });

elemento2 = null; // si el nodo tampoco está en el DOM, el GC puede recolectarlo
// y la entrada de mapaDebil desaparece automáticamente → sin memory leak
```

## Referencia fuerte vs débil

Una **referencia fuerte** es cualquier asignación normal en JS: variable, propiedad de objeto, elemento de array, entrada de Map o Set. Mientras exista al menos una referencia fuerte a un objeto, el GC no lo toca.

Una **referencia débil** no cuenta para el recolector. El motor la trata como "si nadie más lo referencia, puedes recolectarlo". WeakMap y WeakSet implementan referencias débiles a nivel de estructura de datos.

```js
// Demostración conceptual (el GC no es determinista — este código es ilustrativo)
let obj = { dato: 42 };

const m = new Map();
m.set(obj, "fuerte");

const wm = new WeakMap();
wm.set(obj, "débil");

obj = null; // eliminamos la única referencia fuerte externa

// Map sigue manteniendo obj vivo — el GC no puede tocarlo
// WeakMap tiene solo una referencia débil — el GC puede recolectar obj
```

## Restricción de tipo: solo objetos como claves

`WeakMap` solo acepta claves de tipo objeto (incluidos arrays, funciones, objetos DOM, instancias). `WeakSet` solo acepta objetos como valores. Los primitivos (strings, números, booleans, `null`, `undefined`) están prohibidos porque los primitivos no tienen identidad de referencia — no tiene sentido tener una referencia "débil" a un valor inmutable.

```js
const wm = new WeakMap();

wm.set({}, "ok");          // objeto literal — válido
wm.set([], "ok");          // array — válido
wm.set(() => {}, "ok");    // función — válido
wm.set(document.body, {}); // nodo DOM — válido

wm.set("clave", "valor");  // TypeError: Invalid value used as weak map key
wm.set(42, "valor");       // TypeError
wm.set(null, "valor");     // TypeError
```

## Por qué no hay iteración ni `size`

La no-iterabilidad es una consecuencia directa de la semántica débil. Si `WeakMap` expusiera `keys()` o `size`, habría una ventana temporal en la que una iteración podría ver un objeto que el GC elimina a mitad del recorrido — o `size` podría cambiar entre dos instrucciones JS sin que el código haya hecho nada. Para preservar la integridad del modelo de ejecución JS (sin interrupciones por el GC durante una instrucción), se eliminó la iterabilidad por completo.

```js
const wm = new WeakMap();
const ws = new WeakSet();

// Todo esto lanza TypeError o simplemente no existe:
// wm.keys()        → TypeError: wm.keys is not a function
// wm.values()      → TypeError
// wm.forEach(...)  → TypeError
// wm.size          → undefined  (la propiedad no existe en el prototipo)
// ws.size          → undefined

console.log("size" in WeakMap.prototype); // → false
console.log("size" in WeakSet.prototype); // → false
```

## API de WeakMap

```js
const wm = new WeakMap();

// set — añade o actualiza
const key = {};
wm.set(key, 42);
wm.set(key, 99);           // actualiza — misma clave

// get — obtiene el valor
console.log(wm.get(key));  // → 99
console.log(wm.get({}));   // → undefined  (objeto distinto)

// has — comprueba existencia
console.log(wm.has(key));  // → true

// delete — elimina la entrada
wm.delete(key);
console.log(wm.has(key));  // → false
```

## API de WeakSet

```js
const ws = new WeakSet();

const a = {};
const b = {};

ws.add(a);
ws.add(b);
ws.add(a);             // duplicado — ignorado

console.log(ws.has(a)); // → true
console.log(ws.has({})); // → false  (referencia distinta)

ws.delete(a);
console.log(ws.has(a)); // → false
```

## Cómo funciona el GC con referencias débiles

El motor JS usa un algoritmo de mark-and-sweep (marcar y barrer). En cada ciclo de GC:
1. Se marcan como "alcanzables" todos los objetos referenciados fuertemente desde el stack, variables globales y closures.
2. Las entradas de WeakMap y WeakSet **no se marcan** como referencias fuertes durante este paso.
3. Cualquier objeto no marcado (sin referencias fuertes) se barre y su memoria se libera.
4. Las entradas de WeakMap/WeakSet cuya clave fue barrida se eliminan como efecto secundario.

El momento exacto en que esto ocurre no está especificado — depende del motor y de la carga. No se puede forzar el GC desde JS.

> [!tip] Buenas prácticas
> - Usar WeakMap cuando necesitas asociar datos a objetos cuyo ciclo de vida no controlas (nodos DOM, instancias de terceros) — los datos se limpian solos cuando el objeto muere.
> - Usar WeakSet cuando necesitas marcar objetos como "vistos" o "procesados" sin impedir que sean recolectados.
> - No intentar usar WeakMap como cache observable (sin `size`) — para eso, Map o bibliotecas con LRU.

> [!warning] Errores comunes
> - **Intentar iterar un WeakMap/WeakSet:** no existe `forEach`, `keys()`, `values()`, etc. Si necesitas iterar, usa Map/Set.
> - **Usar primitivos como clave de WeakMap:** lanza `TypeError` en tiempo de ejecución. La clave siempre debe ser un objeto.
> - **Asumir que el GC ocurre inmediatamente:** asignar `null` a la variable no libera la entrada del WeakMap al instante. El GC decide cuándo.
> - **Acceder a `wm.size`:** no lanza error pero devuelve `undefined` — la propiedad no existe en el prototipo. No confundir con `Map.size`.

## Notas relacionadas

- [[04 WeakMap y WeakSet/index|WeakMap y WeakSet — índice]]
- [[02 Casos de Uso]]
- [[02 Map/index|Map — índice]]
