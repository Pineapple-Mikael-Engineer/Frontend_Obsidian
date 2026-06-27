---
title: Cadena de Prototipos — Herencia por delegación en tiempo de ejecución
aliases:
  - prototype chain
  - [[Prototype]]
  - cadena prototípica
tags:
  - javascript
  - api/concepto
  - prototipos
  - oop
objeto: Object
tipo: concepto
muta: false
asincrono: false
draft: false
order: 1
---

# Cadena de Prototipos

> [!definicion]
> Cada objeto tiene un slot interno `[[Prototype]]` que apunta a otro objeto o a `null`. Al leer `obj.prop`, si `prop` no existe como propiedad propia de `obj`, el motor busca en `obj.[[Prototype]]`, luego en `obj.[[Prototype]].[[Prototype]]`, y así hasta `null`. Este recorrido es la **cadena de prototipos**. La búsqueda ocurre solo en lectura; escritura y borrado actúan exclusivamente sobre el objeto propio.

```js
const base = { metodo() { return "base"; } };
const medio = Object.create(base);
const hijo = Object.create(medio);

hijo.metodo(); // "base" — recorre hijo → medio → base
hijo.prop = 42; // escribe SOLO en hijo, no sube la cadena

console.log(Object.getPrototypeOf(hijo) === medio); // true
console.log(Object.getPrototypeOf(medio) === base);  // true
console.log(Object.getPrototypeOf(base) === Object.prototype); // true
console.log(Object.getPrototypeOf(Object.prototype)); // null
```

## El algoritmo `[[Get]]`

Cuando el motor ejecuta `obj.prop`, invoca la operación interna `[[Get]](obj, "prop")`:

1. Busca `prop` como propiedad propia de `obj` (con `[[GetOwnProperty]]`).
2. Si la encuentra, retorna su valor.
3. Si no, toma `proto = obj.[[Prototype]]`.
4. Si `proto` es `null`, retorna `undefined`.
5. Repite desde el paso 1 con `proto` como nuevo objeto.

El bucle termina en `null` (fin de la cadena) o al encontrar la propiedad. Nunca se crea ni se copia nada: es pura búsqueda por referencia.

## La cadena estándar de los tipos nativos

```js
// Array
const arr = [1, 2, 3];
// arr → Array.prototype → Object.prototype → null
arr.map    // encontrado en Array.prototype
arr.toString // encontrado en Object.prototype

// Función
function f() {}
// f → Function.prototype → Object.prototype → null
f.call  // encontrado en Function.prototype
f.toString // encontrado en Object.prototype
```

| Tipo | Cadena completa |
|---|---|
| `{}` | `Object.prototype` → `null` |
| `[]` | `Array.prototype` → `Object.prototype` → `null` |
| `function(){}` | `Function.prototype` → `Object.prototype` → `null` |
| `Object.create(null)` | `null` (sin cadena) |

## Shadowing (sombra de propiedad)

Cuando un objeto tiene una propiedad con el mismo nombre que algún nodo de su cadena, la propia **sombrea** a la heredada. La heredada no se modifica ni desaparece.

```js
const proto = { valor: 1 };
const obj = Object.create(proto);

console.log(obj.valor); // 1 — viene del prototipo

obj.valor = 99; // crea propiedad PROPIA en obj, sombrea la del prototipo
console.log(obj.valor);   // 99 — propia
console.log(proto.valor); // 1  — intacta en el prototipo
```

> [!warning]
> El shadowing solo aplica a propiedades de datos. Si la propiedad en el prototipo es un **setter** (definido con `Object.defineProperty` como `set`), asignar `obj.prop = valor` invoca el setter del prototipo en lugar de crear una propiedad propia. Es un comportamiento no obvio que produce bugs silenciosos.

```js
const proto = {};
Object.defineProperty(proto, "x", {
  get() { return this._x; },
  set(v) { this._x = v * 2; }, // setter en el prototipo
});
const obj = Object.create(proto);
obj.x = 5;          // invoca el setter del prototipo, no crea propiedad propia
console.log(obj.x); // 10 — ejecutó el setter
```

## Visualizar la cadena completa

```js
function cadena(obj) {
  const pasos = [];
  let p = obj;
  while (p !== null) {
    pasos.push(p);
    p = Object.getPrototypeOf(p);
  }
  return pasos;
}

cadena([]);
// [ [], Array.prototype, Object.prototype ]
```

## Escritura y borrado no suben por la cadena

```js
const proto = { valor: 10 };
const obj = Object.create(proto);

obj.valor = 99;    // crea propiedad propia en obj
delete obj.valor;  // borra la propia
console.log(obj.valor); // 10 — vuelve a verse la del prototipo

delete obj.valor;  // no hace nada (no existe propia)
// proto.valor sigue intacto
```

## Cómo funciona por dentro

`[[Prototype]]` es un campo del objeto en el heap del motor (en V8 se almacena en el "shape" o "hidden class" del objeto). El algoritmo `[[Get]]` es un bucle simple sobre ese campo. El motor optimiza la búsqueda con inline caches: la primera vez recorre la cadena y guarda la posición de la propiedad; las siguientes invocaciones saltan directamente al nodo correcto sin volver a recorrer. Si se cambia la cadena con `Object.setPrototypeOf`, esas cachés se invalidan.

> [!tip]
> Para leer el prototipo de un objeto, usar siempre `Object.getPrototypeOf(obj)`. La propiedad `__proto__` existe en la mayoría de entornos (es un getter/setter en `Object.prototype`) pero está desaconsejada para código nuevo: no existe en objetos creados con `Object.create(null)` y puede ser sobrescrita.

> [!warning]
> La longitud de la cadena tiene coste en rendimiento. Una cadena de 5–6 niveles de profundidad es inusual y rara vez justificada. Cadenas circulares son imposibles: el motor lanza `TypeError` al intentar establecerlas.

## Notas relacionadas

- [[02 getPrototypeOf y setPrototypeOf]]
- [[03 Propiedades Propias vs Heredadas]]
- [[04 Object.create()]]
- [[05 Funciones Constructoras (new, this)]]
- [[06 Herencia Prototípica]]
