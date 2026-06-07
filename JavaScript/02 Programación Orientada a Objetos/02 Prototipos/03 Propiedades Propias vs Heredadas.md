---
title: Propiedades Propias vs Heredadas — Qué pertenece al objeto y qué viene del prototipo
aliases:
  - own properties
  - inherited properties
  - hasOwnProperty
  - Object.hasOwn
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
---

# Propiedades Propias vs Heredadas

> [!definicion]
> Una **propiedad propia** está definida directamente en el objeto (su descriptor vive en ese objeto). Una **propiedad heredada** se encuentra en algún nodo de su cadena de prototipos. La distinción es fundamental para iterar correctamente, copiar objetos y detectar colisiones con claves prototípicas.

```js
const proto = { heredada: true };
const obj = Object.create(proto);
obj.propia = 42;

"propia"   in obj; // true — la encuentra (propia)
"heredada" in obj; // true — la encuentra (en proto)

Object.hasOwn(obj, "propia");    // true
Object.hasOwn(obj, "heredada");  // false
```

## Métodos para distinguirlas

### `Object.hasOwn(obj, prop)` — ES2022, forma moderna

```js
const obj = { a: 1 };
Object.hasOwn(obj, "a");          // true
Object.hasOwn(obj, "toString");   // false — heredada de Object.prototype
```

Funciona en objetos creados con `Object.create(null)` (sin prototipo), a diferencia de `hasOwnProperty`.

### `obj.hasOwnProperty(prop)` — forma legada

```js
obj.hasOwnProperty("a");        // true
obj.hasOwnProperty("toString"); // false
```

Puede fallar en dos casos: (1) el objeto fue creado con `Object.create(null)` y no tiene ese método; (2) la propiedad `"hasOwnProperty"` fue sobrescrita por el propio objeto.

```js
const puro = Object.create(null);
puro.hasOwnProperty; // undefined → TypeError si se llama

// Forma segura cuando el origen del objeto es desconocido:
Object.prototype.hasOwnProperty.call(obj, "prop");
// o mejor: Object.hasOwn(obj, "prop")
```

## Métodos de enumeración y su alcance

```js
const proto = { y: 2 };
const obj = Object.create(proto);
obj.x = 1;
Object.defineProperty(obj, "oculta", { value: 99, enumerable: false });
```

| Método | Propias | Heredadas | Enumerables | No enumerables |
|---|---|---|---|---|
| `Object.keys(obj)` | sí | no | sí | no |
| `Object.values(obj)` | sí | no | sí | no |
| `Object.entries(obj)` | sí | no | sí | no |
| `Object.getOwnPropertyNames(obj)` | sí | no | sí | sí |
| `Object.getOwnPropertySymbols(obj)` | sí (Symbols) | no | sí | sí |
| `for...in` | sí | sí | sí | no |
| `"prop" in obj` | sí | sí | sí | sí |
| `Object.hasOwn(obj, "prop")` | sí | no | sí | sí |

```js
Object.keys(obj);                   // ["x"]
Object.getOwnPropertyNames(obj);    // ["x", "oculta"]
[...Object.keys(obj)];              // ["x"]

for (const k in obj) {
  console.log(k); // "x", "y" — incluye heredada
}

for (const k in obj) {
  if (Object.hasOwn(obj, k)) console.log(k); // solo "x"
}
```

## El flag `enumerable`

Cada propiedad tiene un descriptor con el campo `enumerable` (booleano). `Object.defineProperty` permite crearlo en `false`; las asignaciones normales crean propiedades con `enumerable: true`.

```js
Object.defineProperty(obj, "id", { value: 1, enumerable: false, writable: false, configurable: false });
Object.keys(obj);                // "id" no aparece
Object.getOwnPropertyNames(obj); // "id" sí aparece
```

## Copiar solo propiedades propias

```js
// Spread — copia propias enumerables
const copia = { ...obj };

// Object.assign — copia propias enumerables de cada fuente
const copia2 = Object.assign({}, obj);

// Copia profunda selectiva
const copiaSel = {};
for (const k of Object.keys(obj)) {
  copiaSel[k] = obj[k];
}
```

> [!warning]
> `{ ...obj }` y `Object.assign` copian solo las propiedades **propias enumerables**. Las propias no enumerables y las heredadas no se copian. Si el prototipo tiene métodos que necesitas, debes establecer el prototipo del destino explícitamente con `Object.create` o `Object.setPrototypeOf`.

## Receta: filtrar `for...in` de forma robusta

```js
function propiasSolo(obj, cb) {
  for (const k in obj) {
    if (Object.hasOwn(obj, k)) cb(k, obj[k]);
  }
}
```

## Cómo funciona por dentro

El descriptor de cada propiedad propia se almacena en la **hidden class** del objeto (en V8) o en la tabla de propiedades del objeto. `[[GetOwnProperty]]` busca solo en esa tabla sin subir por la cadena. `Object.keys` filtra el resultado de `[[OwnPropertyKeys]]` por el flag `enumerable`. El operador `in` combina `[[GetOwnProperty]]` con el recorrido de `[[Prototype]]` hasta `null`.

> [!tip]
> En código que procesa objetos de origen desconocido (p. ej., datos de un servidor deserializados con `JSON.parse`), preferir `Object.hasOwn` sobre `hasOwnProperty` y preferir `Object.keys` sobre `for...in` para evitar iterar propiedades heredadas no deseadas.

## Notas relacionadas

- [[01 Cadena de Prototipos]]
- [[02 getPrototypeOf y setPrototypeOf]]
- [[04 Object.create()]]
- [[01 Objetos Literales/08 Comprobar y Eliminar (in, hasOwnProperty, delete) | in, hasOwnProperty, delete]]
