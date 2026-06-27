---
title: Object.getPrototypeOf / Object.setPrototypeOf — Leer y escribir [[Prototype]]
aliases:
  - getPrototypeOf
  - setPrototypeOf
  - __proto__
tags:
  - javascript
  - api/metodo
  - prototipos
  - oop
objeto: Object
tipo: metodo
retorna: object | null
muta: true
asincrono: false
draft: false
order: 2
---

# `Object.getPrototypeOf` / `Object.setPrototypeOf`

> [!definicion]
> `Object.getPrototypeOf(obj)` retorna el `[[Prototype]]` del objeto: el objeto que actúa como su prototipo, o `null` si no tiene. `Object.setPrototypeOf(obj, proto)` reemplaza el slot `[[Prototype]]` de un objeto existente por `proto` (o `null`). Son la API estándar para inspeccionar y modificar la cadena de prototipos en tiempo de ejecución.

```js
const animal = { respira: true };
const perro = Object.create(animal);

Object.getPrototypeOf(perro) === animal; // true
Object.getPrototypeOf(animal) === Object.prototype; // true
Object.getPrototypeOf(Object.prototype); // null

// Cambiar el prototipo en caliente (raramente justificado)
const gato = {};
Object.setPrototypeOf(gato, animal);
gato.respira; // true — ahora hereda de animal
```

## `Object.getPrototypeOf`

Firma: `Object.getPrototypeOf(obj) → object | null`

Lee el slot `[[Prototype]]` directamente. Si `obj` no es un objeto, en ES6+ lo coerciona con `ToObject`; si el argumento es `null` o `undefined`, lanza `TypeError`.

```js
// Cadenas de tipos nativos
Object.getPrototypeOf([])          === Array.prototype;    // true
Object.getPrototypeOf(function(){}) === Function.prototype; // true
Object.getPrototypeOf({})          === Object.prototype;   // true
Object.getPrototypeOf(Object.create(null)); // null
```

### Recorrido manual de la cadena

```js
function cadenaPrototipos(obj) {
  const cadena = [];
  let p = Object.getPrototypeOf(obj);
  while (p !== null) {
    cadena.push(p);
    p = Object.getPrototypeOf(p);
  }
  return cadena;
}

cadenaPrototipos([]); // [Array.prototype, Object.prototype]
```

### Verificar instancia recorriendo la cadena manualmente

```js
function esInstancia(obj, Clase) {
  let p = Object.getPrototypeOf(obj);
  while (p !== null) {
    if (p === Clase.prototype) return true;
    p = Object.getPrototypeOf(p);
  }
  return false;
}

esInstancia([], Array);   // true
esInstancia([], Object);  // true — Object.prototype está más arriba
esInstancia([], Map);     // false
```

## `Object.setPrototypeOf`

Firma: `Object.setPrototypeOf(obj, proto) → obj`

Retorna el mismo objeto modificado. `proto` debe ser un objeto o `null`; cualquier otro valor lanza `TypeError`.

```js
const base = { tipo: "base" };
const ext = { tipo: "extendido" };
const obj = Object.create(base);

obj.tipo; // "base"
Object.setPrototypeOf(obj, ext);
obj.tipo; // "extendido" — la cadena cambió
```

### `null` como prototipo: mapa puro

```js
const mapa = Object.setPrototypeOf({}, null);
// equivalente a Object.create(null)

"toString" in mapa;       // false — no hay cadena
"hasOwnProperty" in mapa; // false

// Útil para diccionarios donde las claves no colisionan con métodos heredados
mapa["constructor"] = "mi dato"; // seguro
```

> [!warning]
> **`Object.setPrototypeOf` es extremadamente costoso en V8.** Al cambiar el slot `[[Prototype]]` de un objeto, el motor invalida todas las **hidden classes** (shapes) que dependen de ese objeto y recompila las inline caches de todos los sitios de llamada que lo referencian. El coste no es O(1): puede degradar código previamente optimizado a modo no optimizado. En código de rendimiento crítico (bucles calientes, estructuras de datos frecuentemente accedidas) se debe evitar por completo. Preferir `Object.create(proto)` al momento de creación.

## `__proto__` (legado)

`__proto__` es un getter/setter definido en `Object.prototype` que expone `[[Prototype]]`. Incluida en el Anexo B del estándar (compatibilidad web, no código nuevo).

```js
const obj = {};
obj.__proto__ = { x: 1 };
obj.x; // 1

// Problema 1: no existe en Object.create(null)
const puro = Object.create(null);
puro.__proto__; // undefined (no es getter, es una clave propia si se asigna)

// Problema 2: la propiedad puede ser sobrescrita
```

| API | Estándar | Existe en `Object.create(null)` | Recomendada |
|---|---|---|---|
| `Object.getPrototypeOf` | ES5 | sí | sí |
| `Object.setPrototypeOf` | ES6 | sí | con precaución |
| `__proto__` getter | Anexo B | no | no (legado) |

## Cómo funciona por dentro

`Object.getPrototypeOf` es una envoltura trivial sobre la operación interna `[[GetPrototypeOf]]`, que lee un campo del objeto en el heap. Costo O(1), sin efectos secundarios.

`Object.setPrototypeOf` invoca `[[SetPrototypeOf]]`. V8 implementa objetos con **hidden classes**: dos objetos que comparten la misma forma (mismas propiedades en el mismo orden) comparten una hidden class y son optimizables juntos. Al cambiar `[[Prototype]]`, el objeto adquiere una nueva shape y el motor debe reconstruir el grafo de dependencias de optimización para todos los sitios que usaban ese objeto. El coste escala con el número de sitios de llamada optimizados.

> [!tip]
> Para crear un objeto con prototipo específico, usar `Object.create(proto)` en lugar de crear el objeto primero y luego llamar a `setPrototypeOf`. `Object.create` establece `[[Prototype]]` en la construcción, sin romper optimizaciones.

## Notas relacionadas

- [[01 Cadena de Prototipos]]
- [[04 Object.create()]]
- [[03 Propiedades Propias vs Heredadas]]
